<template>
  <div id="app">
    <search></search>
    <ubike-table 
    :parent-message="uBikeStopsFilter"
    ></ubike-table>
    <pagination
    ></pagination>
  </div>
</template>

<script>
import UbikeTable from "./components/UbikeTable.vue";
import Search from "./components/Search.vue";
import Pagination from "./components/Pagination.vue";
export default {
  name: "App",
  components: {
    UbikeTable,
    Search,
    Pagination
  },
  data() {
    return {
      uBikeStops: [],
     //currentPage:1
    };
  },
  computed: {
    uBikeStopsFilter() {
      return this.uBikeStops;
    },
  },
  methods: {
    // selfUpdate(val) {
    //   this.currentPage = val;
    // }
  },  
  created() {
    fetch("https://tcgbusfs.blob.core.windows.net/blobyoubike/YouBikeTP.gz")
      .then((res) => res.json())
      .then((json) => {
        const stops = Object.keys(json.retVal).map((key) => json.retVal[key]);
        this.uBikeStops = stops;
      });
  },
};
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>



