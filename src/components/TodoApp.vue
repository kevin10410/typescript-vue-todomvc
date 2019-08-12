<template lang="pug">
  section.todoapp
    header.header
      h1 todos
      input.new-todo(
        @keyup.enter="createTodo()"
        autofocus
        v-model="newTodoTitle"
        placeholder="What needs to be done?")
    section.main(v-if="todos.length")
      input.toggle-all(type="checkbox")
      label(for="toggle-all"
        @click="toggleAll") Mark all as complete
      ul.todo-list
        div(v-for="(todo, index) in todosInView")
          todo-item(
            :todo="todo"
            @removeSelf="removeTodo(index)"
            @toggleCompleted="toggleCompleted(index)")
    todo-footer(
      v-if="todos.length"
      :itemsLeft="remaining.length"
      :currentView="currentView"
      :clearCompleted="clearCompleted")
</template>

<script lang="ts">
import TodoFooter from './TodoFooter.vue';
import TodoHeader from './TodoHeader.vue';
import TodoItem from './TodoItem.vue';

import Vue from 'vue';
import { AppState, Todo } from '../models';

export default Vue.extend({
  components: {
    TodoItem,
    TodoFooter,
  },

  props: ['currentView'],

  data() {
    const initialState: AppState = {

      // Input box content.
      newTodoTitle: '',

      // Current todo items.
      todos: [
        { completed: false, title: 'Use Vue with TypeScript' },
      ],
    };

    return initialState;
  },
  methods: {
    /**
     * Adds a new Todo to this instance's list, and reset the title.
     */
    createTodo() {
      const title = this.newTodoTitle.trim();
      if (!title) {
        return;
      }

      this.todos.push({
        completed: false,
        title,
      });

      this.newTodoTitle = '';
    },

    /**
     * Removes the Todo at the given index.
     */
    removeTodo(index: number) {
      if (index >= this.todos.length) {
        throw new Error(`Index deletion at ${index} greater than ${this.todos.length}`);
      }

      this.todos.splice(index, 1);
    },

    /**
     * Toggle the Todo's `completed` status at a given position.
     */
    toggleCompleted(index: number) {
      const current = this.todos[index];
      this.todos.splice(index, 1, {
        ...current,
        completed: !current.completed
      });
    },

    /**
     * Toggle the Todo's `completed` status at a given position.
     */
    clearCompleted() {
      this.todos = this.remaining;
    },

    /**
     * Toggles all todos in one wipe.
     * The `completed` status for all todos is set to `true` if any todo is not completed.
     * It is set to `false` otherwise.
     */
    toggleAll() {
      const stateForAll = this.completed.length !== this.todos.length;

      for (const todo of this.todos) {
        todo.completed = stateForAll;
      }
    }
  },

  computed: {
    todosInView(): Todo[] {
      switch (this.currentView) {
        case 'completed':
          return this.completed;
        case 'active':
          return this.remaining;
        case 'all':
        default:
          return this.todos;
      }
    },
    completed(): Todo[] {
      return this.todos.filter(isCompleted);
    },
    remaining(): Todo[] {
      return this.todos.filter(isNotCompleted);
    },
  },
});

function isCompleted(todo: Todo) {
  return todo.completed;
}

function isNotCompleted(todo: Todo) {
  return !todo.completed;
}
</script>