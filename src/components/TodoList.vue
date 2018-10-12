<template>
  <div>
    <input type="text" class="todo-input" placeholder="Add Items to Basket" v-model="newTodo" @keyup.enter="addTodo">
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
      <todo-item v-for="todo in todosFiltered" :key="todo.id" :todo="todo" :checkAll="!anyRemaining">
      </todo-item>
    </transition-group>

    <div class="extra-container">
      <todo-check-all></todo-check-all>
      <todo-items-remaining></todo-items-remaining>
    </div> <!-- end extra-container -->

    <div class="extra-container">
      <todo-filtered></todo-filtered>

      <div>
        <transition name="fade">
          <todo-clear-completed></todo-clear-completed>
        </transition>
      </div>
    </div> <!-- end extra-container -->
  </div>
</template>

<script>
  import TodoItem from './TodoItem'
  import TodoItemsRemaining from './TodoItemsRemaining'
  import TodoCheckAll from './TodoCheckAll'
  import TodoFiltered from './TodoFiltered'
  import TodoClearCompleted from './TodoClearCompleted'

  export default {
    name: 'todo-list',
    components: {
      TodoItem,
      TodoItemsRemaining,
      TodoCheckAll,
      TodoFiltered,
      TodoClearCompleted,
    },
    data() {
      return {
        newTodo: '',
        idForTodo: 3,
      }
    },
    computed: {
      anyRemaining() {
        return this.$store.getters.anyRemaining
      },
      todosFiltered() {
        return this.$store.getters.todosFiltered
      }
    },
    methods: {
      addTodo() {
        if (this.newTodo.trim().length == 0) {
          return
        }

        this.$store.dispatch('addTodo', {
          id: this.idForTodo,
          title: this.newTodo,
        })

        this.newTodo = ''
        this.idForTodo++
      },
    }
  }

</script>

<style lang="scss">
  @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");
  html,body{
    padding: 0;
    height: 100%;
    background: #fcfcfc;
  }
  .todo-input {
    width: 100%;
    padding: 0 0 0 20px;
    background: #f3f3f3;
    border: 0;
    height: 40px;
    font-weight: 300;
    margin: 4px 0;
    font-size: 13px;
    letter-spacing: .7px;
    border-radius: 3px;
    box-shadow: inset 0px 1px 8px 0px rgba(0, 0, 0, 0.1);

    &:focus {
      outline: 0;
    }
  }

  .todo-item {
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    animation-duration: 0.3s;
    font-size: 18px;
  }

  .remove-item {
    cursor: pointer;
    margin-left: 14px;

    &:hover {
      color: black;
    }
  }

  .todo-item-left {
    display: flex;
    align-items: center;
    cursor: pointer;
  }

  .todo-item-label {
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
  }

  .todo-item-edit {
    font-size: 18px;
    color: #2c3e50;
    margin: 5px 10px 5px 12px;
    width: 100%;
    padding: 5px 0 5px 10px;
    border: 1px solid #ccc; //override defaults
    border-radius: 3px;
    font-family: "Avenir", Helvetica, Arial, sans-serif;

    &:focus {
      outline: none;
    }
  }

  .todo-item-checkbox {
    display: none;
  }
  .todo-item-checkbox:checked ~ .sudo-checkbox {
    background-color: #323a45;
    border-color: #323a45;
    &:after {
      content: '';
      height: 8px;
      width: 4px;
      border: solid #fff;
      transform: rotate(45deg);
      border-width: 0 1px 1px 0;
      display: block;
      position: absolute;
      margin-top: 0px;
      margin-left: 4px;
    }
  }
  .sudo-checkbox{
    height: 15px;
    width: 15px;
    display: inline-block;
    vertical-align: middle;
    margin: -5px 0 0 0;
    line-height: 20px;
    text-align: center;
    background-color: #fff;
    border: 1px solid #c5c3c6;
    border-radius: 2px;
  }

  .completed {
    text-decoration: line-through;
    color: grey;
  }

  .extra-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 14px;
    margin-bottom: 14px;
  }

  button {
    font-size: 14px;
    background-color: white;
    appearance: none;
    border-radius: 3px;
    border:1px solid lightgrey;
    padding: 5px 10px;
    margin-right: 5px;
    cursor: pointer;
    &:hover {
      background-color: #323a45;
      border-color: #323a45;
      color: #fff;
    }

    &:focus {
      outline: none;
    }
  }

  .active {
    background-color: #323a45;
    border-color: #323a45;
    color: #fff;
  }

  // CSS Transitions
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.2s;
  }

  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }

</style>
