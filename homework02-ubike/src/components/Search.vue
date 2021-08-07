<template>
  <p>站點名稱搜尋: <input type="text" v-model="searchSna" /></p>
</template>

<script>
export default {
  name: "Search",
  data() {
    return {
      searchSna: "",
    };
  },
  watch: {
    searchSna(val) {
      //透過 this.$emit 發送 update 事件
      this.$emit("onSearchText", val);
    },
  },
};
</script>

<style lang="scss" scoped>
</style>