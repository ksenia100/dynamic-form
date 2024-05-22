<template>
  <div>
    <DynamicForm />
  </div>
</template>

<script>
import { defineComponent } from 'vue';
import DynamicForm from './components/DynamicForm.vue';

export default defineComponent({
  name: 'App',
  components: {
    DynamicForm,
  },
});
</script>

<style>
</style>
