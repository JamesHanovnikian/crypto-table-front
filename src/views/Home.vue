<template>
  <div class="home">
    <h2>  Currency Table </h2> 
    <!-- {{ currencies }} -->
  
  <table class="table table-striped">
    <thead>
      <tr>
        <th v-on:click="sortList('name')"> Name </th> 
        <th v-on:click="sortList('symbol')"> Symbol  </th> 
        <th v-on:click="sortList('market_cap_usd')"> Market Cap (USD) </th> 
        <th v-on:click="sortList('price_usd')"> Price USD  </th> 
        <th v-on:click="sortList('change_percent_24hr')"> Change % 24 Hr  </th> 
      </tr>
    </thead>
    <tbody>
      <tr v-for="currency in currencies">
        <td>{{ currency.name }}</td>
        <td>{{ currency.symbol }}</td>
        <td>{{ currency.market_cap_usd }}</td>
        <td>{{ currency.price_usd }}</td>
        <td>{{ currency.change_percent_24hr }}</td>
      </tr>
    </tbody>
  </table>
  
  
  
  <!-- <table class="table table-striped">
  <thead>
    <tr>
      <th v-for="column in columnNames()"> {{ column }}</th>
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
</table>  -->
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
      sortedData: [],
      sortedbyASC: true,
      search: "",
      currencies: [],
    };
  },
  created() {
    this.indexCurrencies();
  },
  methods: {
    indexCurrencies: function () {
      axios.get("/currencies").then((response) => {
        console.log("currencies loaded", response);
        this.currencies = response.data;
        this.sortedData = response.data;
      });
    },
    sortList: function (sortBy) {
      if (this.sortedbyASC) {
        this.sortedData.sort((x, y) => (x[sortBy] > y[sortBy] ? -1 : 1));
        this.sortedbyASC = false;
      } else {
        this.sortedData.sort((x, y) => (x[sortBy] < y[sortBy] ? -1 : 1));
        this.sortedbyASC = true;
      }
    },
  },
};
</script>
