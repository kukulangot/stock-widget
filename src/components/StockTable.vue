<template>
    <table>
      <thead>
        <tr>
          <th>Stock Code</th>
          <th>Last</th>
          <th>Vol</th>
          <th>Buy</th>
          <th>Buy Vol</th>
          <th>Sell</th>
          <th>Sell Vol</th>
          <th>+/-</th>
          <th>% Chg</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="stock in data" :key="stock.stockcode">
          <td>{{ stock.stockcode }}</td>
          <td :class="{ flash: stock.last !== stock.previousLast }">{{ stock.last }}</td>
          <td>{{ stock.volume }}</td>
          <td>{{ stock.buy_price }}</td>
          <td>{{ stock.buy_volume }}</td>
          <td>{{ stock.sell_price }}</td>
          <td>{{ stock.sell_volume }}</td>
          <td :style="{ color: stock.last - stock.previous < 0 ? 'red' : 'green' }">
            {{ (stock.last - stock.previous).toFixed(2) }}
          </td>
          <td :style="{ color: stock.last - stock.previous < 0 ? 'red' : 'green' }">
            {{ (100 * (stock.last - stock.previous) / stock.previous).toFixed(2) }}%
          </td>
        </tr>
      </tbody>
    </table>
</template>
  
<script>

export default {
    props: ['data'],
    data() {
      return {
        previousData: []
      };
    },
    watch: {
      data() {
        this.previousData = this.data;
      }
    }
  };
  </script>
  
  <style>
table {
    width: 100%; /* Make the table fill the width of its container */
    margin: 0 auto; /* Center the table horizontally */
}
th, td {
        padding: 8px;
        text-align: center; /* Center the content */
        border-bottom: 1px solid #ddd;
    }
    th {
        background-color: #f2f2f2;
    }
  </style>
  