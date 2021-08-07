<template>
  <div id="app">
    <Search @onSearchText="searchTextChange" />

    <UbikeTable 
      :ubikeStops = "ubikeStops" 
      :countOfPage = "count_of_page"
      :searchText = "searchText"
      :currentPage = "currentPage"
      @updatePage = "updatePage"
    />

    <!-- 頁籤 -->
    <Pagination 
      :countOfPage = "count_of_page"
      :currentPage = "currentPage"
      @updatePage = "updatePage"
    />
  </div>
</template>

<script>
// 引入 bootstrap.css
import "bootstrap/dist/css/bootstrap.css";

import Search from "./components/Search.vue";
import UbikeTable from './components/UbikeTable.vue';
import Pagination from './components/Pagination.vue';

// 單頁顯示筆數
const COUNT_OF_PAGE = 10;

export default {
  components: {
    Search,
    UbikeTable,
    Pagination
  },
  data() {
    return {
      count_of_page: COUNT_OF_PAGE,
      ubikeStops: [],
      searchText: "",
      currentPage: 1,
    };
  },
  computed: {
    filtedUbikeStops() {
      // 過濾搜尋
      return this.ubikeStops.length === 0
        ? []
        : this.ubikeStops.filter((d) => d.sna.includes(this.searchText));
    },
  },
  methods: {
    searchTextChange(snaText) {
      this.searchText = snaText;
    },
    updatePage(page) {
      this.currentPage = page;
    }
  },
  created() {
    // 欄位說明請參照:
    // http://data.taipei/opendata/datalist/datasetMeta?oid=8ef1626a-892a-4218-8344-f7ac46e1aa48

    // sno：站點代號、 sna：場站名稱(中文)、 tot：場站總停車格、
    // sbi：場站目前車輛數量、 sarea：場站區域(中文)、 mday：資料更新時間、
    // lat：緯度、 lng：經度、 ar：地(中文)、 sareaen：場站區域(英文)、
    // snaen：場站名稱(英文)、 aren：地址(英文)、 bemp：空位數量、 act：全站禁用狀態

    fetch("https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.gz")
      .then((res) => res.json())
      .then((res) => {
        // 將 json 轉陣列後存入 this.ubikeStops
        this.ubikeStops = Object.keys(res.retVal).map((key) => res.retVal[key]);
      });
  },
};
</script>

<style>
#app {
  padding: 0.5rem;
}

.fa-sort {
  color: #aaa;
}
</style>