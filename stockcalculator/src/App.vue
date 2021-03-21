<template>
  <v-app>
    <v-main>
      <Calculator
        v-if="price > 0"
        :price="price"
        :currentPrice="currentPrice"
        :fiftyTwoWeekHigh="fiftyTwoWeekHigh"
        :fiftyTwoWeekLow="fiftyTwoWeekLow"
        :taxrate="taxrate"
        :exchangeRate="exchangeRate"
      ></Calculator>
      <Cat v-else></Cat>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
import Calculator from "./components/Calculator.vue";
import Cat from "./components/Cat.vue";

export default {
  name: "App",
  components: {
    Calculator,
    Cat,
  },
  data: () => ({
    price: "",
    currentPrice: "",
    fiftyTwoWeekHigh: "",
    fiftyTwoWeekLow: "",
    taxrate: "42",
    exchangeRate: "",
  }),
  props: ["name", "img"],
  mounted() {
    const options = {
      method: "GET",
      url:
        "https://apidojo-yahoo-finance-v1.p.rapidapi.com/stock/v2/get-summary",
      params: { symbol: "7351.T" },
      headers: {
        "x-rapidapi-key": "0dbab4d9fbmshd483c3c870cd761p195c5cjsn707907fcd2d7",
        "x-rapidapi-host": "apidojo-yahoo-finance-v1.p.rapidapi.com",
      },
    };

    axios
      .request(options)
      .then((response) => {
        this.currentPrice = response.data.price.regularMarketPrice.raw;
        this.fiftyTwoWeekHigh =
          response.data.summaryDetail.fiftyTwoWeekHigh.raw;
        this.fiftyTwoWeekLow = response.data.summaryDetail.fiftyTwoWeekLow.raw;
        this.price = this.currentPrice;
      })
      .catch((error) => {
        console.error(error);
      });
  },

  created() {
    const getRate = {
      method: "GET",
      url: "https://api.exchangeratesapi.io/latest",
    };

    axios
      .request(getRate)
      .then((response) => {
        this.exchangeRate = response.data.rates.JPY;
      })
      .catch((error) => {
        console.error(error);
      });
  },
};
</script>
