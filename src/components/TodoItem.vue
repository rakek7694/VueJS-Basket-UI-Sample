<template>
  <div class="todo-item">
    <div class="todo-item-left">
        <label>
          <input type="checkbox" v-model="completed" @change="doneEdit" class="todo-item-checkbox">
          <div class="sudo-checkbox"></div>
        </label>
        <div v-if="!editing" @dblclick="editTodo" class="todo-item-label" :class="{ completed : completed }">{{ title }}</div>
        <input v-else class="todo-item-edit" type="text" v-model="title" @blur="doneEdit" @keyup.enter="doneEdit" @keyup.esc="cancelEdit" v-focus>
    </div> 
    <!-- end bakset-add-item-left -->
    <div>
      <span class="remove-item" @click="removeTodo(todo.id)">
        &times;
      </span>
    </div>
  </div> 
  <!-- end basket-add-item -->
</template>

<script>
export default {
  name: 'todo-item',
  props: {
    todo: {
      type: Object,
      required: true,
    },
    checkAll: {
      type: Boolean,
      required: true,
    }
  },
  data() {
    return {
      'id': this.todo.id,
      'title': this.todo.title,
      'completed': this.todo.completed,
      'editing': this.todo.editing,
      'beforeEditCache': '',
    }
  },
  watch: {
    checkAll() {
      this.completed = this.checkAll ? true : this.todo.completed
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  methods: {
    // Remove Item From Basket
    removeTodo(id) {
      this.$store.dispatch('deleteTodo', id)
    },
    // Edit Item in Basket
    editTodo() {
      this.beforeEditCache = this.title
      this.editing = true
    },
    // Update
    doneEdit() {
      if (this.title.trim() == '') {
        this.title = this.beforeEditCache
      }
      this.editing = false
      this.$store.dispatch('updateTodo', {
        'id': this.id,
        'title': this.title,
        'completed': this.completed,
        'editing': this.editing,
      })
    },
    // Cancel Edit
    cancelEdit() {
      this.title = this.beforeEditCache
      this.editing = false
    },
    
    pluralize() {
      eventBus.$emit('pluralize')
    },

    handlePluralize() {
      this.title = this.title + 's'
      this.$store.dispatch('updateTodo', {
        'id': this.id,
        'title': this.title,
        'completed': this.completed,
        'editing': this.editing,
      })
    }
  }
}
</script>

