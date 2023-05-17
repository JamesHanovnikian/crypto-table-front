<template>
  <div class="home">
    <h2>  Currency Table </h2> 

      <table class="table table-striped">
  <thead>
    <tr>
      <th>Id </th>
      <th> Name  </th>
      <th> Symbol </th>
      <th> Market Cap (USD) </th> 
      <th> Price (USD) </th> 
      <th> Change % in 24 Hours </th> 
    </tr>
  </thead>
  <tbody>
    <tr v-for="currency in currencies">
       <th scope="row">{{ currency.id }}</th>  
       <td>{{ currency.name }}</td> 
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
table,
th,
td {
  border: 1px solid black;
}
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      currencies: [],
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
  },
};
</script>
