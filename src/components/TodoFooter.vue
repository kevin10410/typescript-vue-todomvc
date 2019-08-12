<template lang="pug">
  footer.footer
    span.todo-count
      strong {{itemsLeft}} {{itemSingularOrPlural}} left
    ul.filters
      li
        router-link(to="/"
          :class="{ selected: !isActive && !isCompleted }") All
      li
        router-link(to="/active"
          :class="{ selected: isActive }") Active
      li
        router-link(to="/completed"
          :class="{ selected: isCompleted }") Completed
    button.clear-completed(
      @click="clearCompleted()") Clear completed
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  props: ['itemsLeft', 'clearCompleted', 'currentView'],
  computed: {
    itemSingularOrPlural(): string {
      return this.itemsLeft === 1 ? 'item' : 'items';
    },
    isActive(): boolean {
      return this.currentView === 'active';
    },
    isCompleted(): boolean {
      return this.currentView === 'completed';
    }
  }
});
</script>