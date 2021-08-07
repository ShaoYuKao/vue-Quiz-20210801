<template>
  <div>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>#</th>
          <th>場站名稱</th>
          <th>場站區域</th>
          <th>
            <div @click="setSort('sbi')">
              目前可用車輛
              <i
                v-if="currentSort === 'sbi'"
                :class="{
                  'fa-sort-asc': !isSortDesc,
                  'fa-sort-desc': isSortDesc,
                }"
                class="fa"
                aria-hidden="true"
              ></i>
              <i v-else class="fa fa-sort" aria-hidden="true"></i>
            </div>
          </th>
          <th>
            <div @click="setSort('tot')">
              總停車格
              <i
                v-if="currentSort === 'tot'"
                :class="{
                  'fa-sort-asc': !isSortDesc,
                  'fa-sort-desc': isSortDesc,
                }"
                class="fa"
                aria-hidden="true"
              ></i>
              <i v-else class="fa fa-sort" aria-hidden="true"></i>
            </div>
          </th>
          <th>資料更新時間</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="s in slicedUbikeStops" :key="s.sno">
          <td>{{ s.sno }}</td>
          <td>{{ s.sna }}</td>
          <td>{{ s.sarea }}</td>
          <td>{{ s.sbi }}</td>
          <td>{{ s.tot }}</td>
          <td>{{ timeFormat(s.mday) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "UbikeTable",
  props: {
    ubikeStops: {
      type: Array,
    },
    searchText: {
      type: String,
    },
    currentPage: {
      type: Number,
    },
    updatePage: {
      type: Function
    }
  },
  data() {
    return {
      countOfPage: 10,
      currentSort: "sno",
      isSortDesc: false,
    };
  },
  computed: {
    sortedUbikeStops() {
      // 拿過濾的結果做排序
      const filtedStops = [...this.$parent.filtedUbikeStops];

      return this.isSortDesc
        ? filtedStops.sort((a, b) => b[this.currentSort] - a[this.currentSort])
        : filtedStops.sort((a, b) => a[this.currentSort] - b[this.currentSort]);
    },
    slicedUbikeStops() {
      // 將排序的結果做分頁切割
      const start = (this.currentPage - 1) * this.countOfPage;
      const end =
        start + this.countOfPage <= this.sortedUbikeStops.length
          ? start + this.countOfPage
          : this.sortedUbikeStops.length;

      return this.sortedUbikeStops.slice(start, end);
    }
  },
  watch: {
    sortedUbikeStops() {
      // 當搜尋條件、排序變更時，強制切到第一頁
      this.$emit("updatePage", 1);
    },
  },
  methods: {
    timeFormat(val) {
      // 時間格式
      const pattern = /(\d{4})(\d{2})(\d{2})(\d{2})(\d{2})(\d{2})/;
      return val.replace(pattern, "$1/$2/$3 $4:$5:$6");
    },
    setSort(sortType) {
      // 切換排序
      if (sortType === this.currentSort) {
        this.isSortDesc = !this.isSortDesc;
      } else {
        this.currentSort = sortType;
        this.isSortDesc = false;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
</style>