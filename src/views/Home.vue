<template>
  <div class="home">
    <h2>  Currency Table </h2> 
  <table class="table table-striped">
  <thead>
    <tr>
      <th v-on:click="sortPrice(column)" v-for="column in columnNames()"> {{ column }}</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="currency in currencies">
       <th scope="row">{{ currency.id }}</th>  
       <td> {{ currency.name }} </td> 
       <td>{{  currency.symbol }}</td>  
       <td>{{  currency.market_cap_usd }} </td>
       <td>{{  currency.price_usd }} </td>
       <td>{{  currency.change_percent_24hr }} </td> 
    </tr>
   </tbody>
</table> 
  </div>
</template>

<style>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      currencies: [],
      column: [],
      currentColumn: {},
    };
  },

  created: function () {
    this.indexCurrencies();
  },
  methods: {
    indexCurrencies: function () {
      axios.get("/currencies").then((response) => {
        console.log("currencies loaded", response);
        this.currencies = response.data;
      });
    },
    columnNames: function () {
      const names = new Set();
      for (const row of this.currencies) {
        for (const key of Object.keys(row)) {
          names.add(key);
        }
      }
      return names;
    },
    sortPrice: function (column) {
      axios.post("/by_price", this.currentColumn).then((response) => {
        this.currentColumn.column = column;
        if (this.currentColumn.direction == null) {
          this.currentColumn.direction = true;
        } else if (this.currentColumn.direction == false) {
          this.currentColumn.direction = true;
        } else if (this.currentColumn.direction == true) {
          this.currentColumn.direction = false;
        }
        console.log(this.currentColumn, response.data);
        this.currencies = response.data;
      });
    },
  },
};
</script>
