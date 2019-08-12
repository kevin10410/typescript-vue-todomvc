<template lang="pug">
  li(:class="{ completed: todo.completed }")
    div.view
      input.toggle(
        type="checkbox"
        v-model:checked="todo.completed")
      label {{todo.title}}
      button.destroy(
        @click="$emit('removeSelf')")
    input.edit(value="Create a TodoMVC template")
</template>

<script lang="ts">
export default {
  props: ['todo']
};
</script>