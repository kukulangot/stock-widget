<template>
  <div :class="theme">
    <div class="select-button select-button-container">
    <select v-model="selectedExchange" @change="fetchData">
      <option value="0">SGX</option>
      <option value="2">Bursa</option>
      <option value="3">Nasdaq</option>
    </select>
  </div>
    <div class="button-grid">
    <button @click="currentList = 0" :class="{ active: currentList === 0 }">Top Volume</button>
    <button @click="currentList = 1" :class="{ active: currentList === 1 }">Top Gainers</button>
    <button @click="currentList = 2" :class="{ active: currentList === 2 }">Top Losers</button>
  </div>

    <stock-table :data="tableData"></stock-table>

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
.button-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}

button {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 10px;
  background-color: #f8f8f8;
  text-align: center;
  cursor: pointer;
  transition: background-color 0.3s;
}

button.active {
  background-color: #e0e0e0;
}
.select-button {
  display: inline-block;
  position: relative;
  overflow: hidden;
  border: 1px solid #ccc;
  border-radius: 4px;
}

select {
  appearance: none;
  padding: 6px 10px;
  font-size: inherit;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: transparent;
  cursor: pointer;
}

select:focus {
  outline: none;
}

/* Style the arrow icon */
.select-button::after {
  content: '\25BE'; /* Unicode character for down arrow */
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
  pointer-events: none;
}
.select-button-container {
  width: 100%; /* Set the width of the container */
  text-align: right; /* Align the content to the right */
}
</style>
