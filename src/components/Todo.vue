<template>
  <li class="todo" :class="{completed: todo.done,editing: editing}">
    <div class="view">
      <input class="toggle" type="checkbox" :checked="todo.done" @change="toggleTodo({todo: todo})">
      <label v-text="todo.text" @dblclick="editing = true"></label>
      <button class="destroy" @click="deleteTodo({todo: todo})"></button>
    </div>
    <input class="edit"
      v-show="editing"
      v-focus="editing"
      :value="todo.text"
      @keyup.enter="doneEdit"
      @keyup.esc="cancelEdit"
      @blur="doneEdit">
  </li>
</template>

<script>
export default {
  props: ['todo'],
  data () {
    return {
      editing: false
    }
  },
  directives: {
    focus (el,{value},{context}){
      if(value){
        context.$nextTick(() => {
          el.focus();
        })
      }
    }
  },
  methods: {
    toggleTodo ({todo}) {
      todo.done = !todo.done
    },
    deleteTodo ({todo}) {
      this.$emit('deleteTodo',todo)
    },
    doneEdit (e) {
      const val = e.target.value;
      const { todo } = this;
      if(!val){
        this.deleteTodo({todo})
      }else if(this.editing){
        this.editTodo({
          todo,
          val
        })
        this.editing = false
      }
    },
    cancelEdit (e) {
      e.target.value = this.todo.text;
      this.editing = false;
    },
    editTodo (todo,val) {
      todo.text = val;
    }
  }
}
</script>

