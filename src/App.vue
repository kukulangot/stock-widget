<template>
  <div :class="theme">
    <select v-model="selectedExchange" @change="fetchData">
      <option value="0">SGX</option>
      <option value="2">Bursa</option>
      <option value="3">Nasdaq</option>
    </select>

    <div>
      <button @click="currentList = 0">Top Volume</button>
      <button @click="currentList = 1">Top Gainers</button>
      <button @click="currentList = 2">Top Losers</button>
    </div>

    <stock-table :data="tableData"></stock-table>

    <button @click="toggleTheme">Toggle Theme</button>
  </div>
</template>

<script>
import axios from 'axios';
import StockTable from './components/StockTable.vue';

export default {
  components: {
    StockTable
  },
  data() {
    return {
      selectedExchange: 0,
      currentList: 0,
      tableData: [],
      theme: 'light'
    };
  },
  methods: {
    fetchData() {
      const url = `https://livefeed3.chartnexus.com/Dummy/quotes?market_id=${this.selectedExchange}&list=${this.currentList}`;
      axios.get(url).then(response => {
        this.tableData = response.data;
      });
    },
    toggleTheme() {
      this.theme = this.theme === 'light' ? 'dark' : 'light';
    }
  },
  mounted() {
    this.fetchData();
    setInterval(this.fetchData, 5000);
  }
};
</script>

<style>
.light {
  background-color: white;
  color: black;
}
.dark {
  background-color: black;
  color: white;
}
/* Magdagdag ng flashing effect styles */
.flash {
  animation: flash 0.5s;
}
@keyframes flash {
  from { background-color: yellow; }
  to { background-color: inherit; }
}
</style>
