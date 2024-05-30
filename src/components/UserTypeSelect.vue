<template>
  <select v-model="selected">
    <option v-for="type in userTypes" :key="type.sm_user_type_id" :value="type.sm_user_type_id">{{ type.name }}</option>
  </select>
</template>

<script>
export default {
  name: 'UserTypeSelect',
  props: {
    value: Number,
    userTypes: Array,
  },
  data() {
    return {
      selected: this.value,
    };
  },
  watch: {
    selected(newVal) {
      this.$emit('input', newVal);
    },
  },
};
</script>
