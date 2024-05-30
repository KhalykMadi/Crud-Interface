<template>
  <div id="app">
    <div class="container">
      <UserTable/>
    </div>
  </div>
</template>

<script>
import UserTable from './components/UserTable.vue';

export default {
  name: 'App',
  components: {
    UserTable,
  },
};
</script>
