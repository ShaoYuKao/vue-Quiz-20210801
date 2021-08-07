<template>
  <li>
    <label><input v-model="info.isDone" type="checkbox" /> {{ info.title }}</label>
  </li>
</template>

<script>
export default {
  props: {
    'listInfo': {
      type: Object
    }
  },
  data() {
    return {
      info: this.listInfo
    }
  },
};
</script>

<style lang="scss" scoped>
</style>