<template>
  <div>
    <ul>
      <li v-if="pageRangeIndex > 0" @click="changeRange('less')" class="pager">
        <a href="#">&lt;</a>
      </li>
      <li class="pager" v-for="n of maxPage" :key="n">
        <!-- n 從 1 開始計算 -->
        <a @click.prevent="changePage(n)" href="#">{{ (pageRangeIndex*pageRange)+ n }}</a>
      </li>
      <li
        v-if="pageRangeIndex < (totalPageCount/pageCount-1)"
        @click="changeRange('add')"
        class="pager"
      >
        <a href="#">&gt;</a>
      </li>
    </ul>
  </div>
</template>
<script>
import bus from "../bus";
export default {
  data() {
    return {
      pageCount: 10, //一頁10筆
      pageRange: 10, //分頁一次顯示十頁
      pageRangeIndex: 0,
      totalRecordCount: 0,
    };
  },
  computed: {
    totalPageCount() {
      return Math.ceil(this.totalRecordCount / this.pageCount);
    },
    maxPage() {
      var mod = this.totalPageCount % this.pageRange;
      if (this.totalRecordCount === 0) {
        return 0;
      } else if (
        this.pageRangeIndex ===
          Math.ceil(this.totalPageCount / this.pageCount) - 1 &&
        mod != 0
      ) {
        return mod;
      }
      return this.pageRange;
    },
  },
  methods: {
    changePage(val) {
      var s = (this.pageRangeIndex * this.pageCount * this.pageRange) + (val - 1) * this.pageCount;
      bus.emit("jumpPage", {
        startIndex: s,
        endIndex: Number.parseInt(s + this.pageCount),
      });
    },
    changeRange(val) {
      if (val === "add") this.pageRangeIndex = this.pageRangeIndex + 1;
      else if (val === "less") this.pageRangeIndex = this.pageRangeIndex - 1;
    },
  },
  watch: {
    totalRecordCount: function () {
      this.pageRangeIndex = 0;
    },
  },
  created() {
    bus.on("totalDataCount", (msg) => {
      this.totalRecordCount = msg;
    });
  },
  beforeUnmount() {
    bus.off("totalDataCount");
  },
};
</script>    
<style scoped>
.pager {
  float: left;
  display: block;
  width: 20px;
  height: 20px;
  text-align: center;
  line-height: 20px;
  margin-right: 5px;
  padding: 5px;
  border: 1px solid #aaa;
}

.pager a {
  text-decoration: none;
}
</style>