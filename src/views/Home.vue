<template>
  <div class="home">
    <div class="container">
      <h2>  Currency Table </h2> 
      <input v-model="search" class="form-control" placeholder="Filter Based on Ticker" /> 
      {{ search }}
      <table class="table table-striped">
        <thead>
          <tr>
            <th v-on:click="sortList('name')"> <a href="#">  Name </a> </th> 
            <th v-on:click="sortList('symbol')"> <a href="#">  Symbol </a> </th> 
            <th v-on:click="sortList('market_cap_usd')"> <a href="#">  Market Cap (USD) </a>  </th> 
            <th v-on:click="sortList('price_usd')"><a href="#">  Price USD </a>  </th> 
            <th v-on:click="sortList('change_percent_24hr')"> <a href="#"> Change % 24 Hr </a> </th> 
          </tr>
        </thead>
        <tbody>
          <tr v-for="currency in filterList(currencies)">
            <td>{{ currency.name }}</td>
            <td>{{ currency.symbol }}</td>
            <td> $ {{ currency.market_cap_usd.toLocaleString() }}</td>
            <td> $ {{ currency.price_usd.toLocaleString() }}</td>
            <td>{{ currency.change_percent_24hr.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
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
      data: [],
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
        this.data = response.data;
      });
    },
    filterList: function (currencies) {
      if (this.search) {
        return currencies.filter((currency) => {
          return currency.symbol
            .toLowerCase()
            .includes(this.search.toLowerCase());
        });
      }
      return currencies;
    },
    sortList: function (sortBy) {
      if (this.sortedbyASC) {
        this.data.sort((x, y) => (x[sortBy] > y[sortBy] ? -1 : 1));
        this.sortedbyASC = false;
      } else {
        this.data.sort((x, y) => (x[sortBy] < y[sortBy] ? -1 : 1));
        this.sortedbyASC = true;
      }
    },
  },
};
</script>
