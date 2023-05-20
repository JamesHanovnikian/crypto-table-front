<template>
  <div class="home">
    <h2>  Currency Table </h2> 
    <!-- {{ currencies }} -->
  
  <table class="table table-striped">
    <thead>
      <tr>
        <th v-for="column in columns">
          <a v-on:click="sortBy(column)" href="#">
            {{ column }}
          </a>
        </th>
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
      sortKey: "name",
      reverse: false,
      search: "",
      currencies: [],
      sortedCurrencies: [],
      columns: [
        "name",
        "symbol",
        "market_cap_usd",
        "price_usd",
        "change_percent_24hr",
      ],
    };
  },

  created: function () {
    this.indexCurrencies();
    this.sortedData();
  },
  methods: {
    indexCurrencies: function () {
      axios.get("/currencies").then((response) => {
        console.log("currencies loaded", response);
        this.currencies = response.data;
      });
    },
    sortedData: function () {
      var sortCurrencies = this.currencies.sort((x, y) =>
        x[this.sortKey] < y[this.sortKey]
          ? 1
          : x[this.sortKey] < y[this.sortKey]
          ? -1
          : 0
      );
      this.sortedCurrencies = sortCurrencies;
      console.log(this.sortedCurrencies);
    },
    sortBy: function (sortKey) {
      this.reverse = (this.sortKey = sortKey) ? !this.reverse : false;
      console.log(this.reverse);
      this.sortKey = sortKey;
      console.log(this.sortKey);
    },
  },
};
</script>
