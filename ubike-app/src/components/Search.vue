<template>
  <div>
    <p>
      站點名稱搜尋:
      <input type="text" v-model.trim="searchName" />
      <button @click="txtChanged">Update</button>
    </p>
  </div>
</template>
<script>
import bus from '../bus'
export default {
  data() {
    return {
      searchName: "",
    };
  },
  methods: {
    txtChanged() {
      bus.emit('search', this.searchName);
    },
  }  
};
</script>