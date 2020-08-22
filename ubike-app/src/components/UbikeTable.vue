<template>
  <div>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>#</th>
          <th>場站名稱</th>
          <th>場站區域</th>
          <th>
            目前可用車輛
            <i @click="sortcolumn(1,'sbi')" class="fa fa-sort-asc" aria-hidden="true"></i>
            <i @click="sortcolumn(-1,'sbi')" class="fa fa-sort-desc" aria-hidden="true"></i>
          </th>
          <th>
            總停車格
            <i @click="sortcolumn(1,'tot')" class="fa fa-sort-asc" aria-hidden="true"></i>
            <i @click="sortcolumn(-1,'tot')" class="fa fa-sort-desc" aria-hidden="true"></i>
          </th>
          <th>資料更新時間</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="s in uBikeStopsShow" :key="s">
          <td>{{ s.sno }}</td>
          <td>{{ s.sna }}</td>
          <td>{{ s.sarea }}</td>
          <td>{{ s.sbi }}</td>
          <td>{{ s.tot }}</td>
          <td>{{ s.mday }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import bus from "../bus";
export default {
  props: {
    parentMessage: [],
  },
  data() {
    return {
      searchNameText: "",
      pageCount: 10, //一頁10筆
      startIndex: 0,
      endIndex: 10,
    };
  },
  computed: {
    ShowList() {
      return this.parentMessage;
    },

    uBikeStopsFilter() {
      var result = [];
      if (this.searchNameText.length != 0) {
        result = this.ShowList.filter((d) =>
          d.sna.toLowerCase().includes(this.searchNameText.toLowerCase())
        );
      } else {
        result = this.ShowList;
      }
      return result;
    },
    uBikeStopsShow() {
      return this.uBikeStopsFilter.slice(this.startIndex, this.endIndex);
    },
  },
  methods: {
    sortcolumn(val, col) {
      return this.ShowList.sort((a, b) => (a[col] - b[col]) * val);
    },
  },
  watch: {
    uBikeStopsFilter: function () {
      this.startIndex = 0;
      this.endIndex = this.startIndex + this.pageCount;
      bus.emit("totalDataCount", this.uBikeStopsFilter.length);
    },
  },
  created() {
    bus.on("search", (msg) => {
      this.searchNameText = msg;
    });
    bus.on("jumpPage", (val) => {
      this.startIndex = val.startIndex;
      this.endIndex = val.endIndex;
    });
  },
  beforeUnmount() {
    bus.off("search");
    bus.off("jumpPage");
  },
};
</script>    