<template>
  <v-app>
    <v-main>
      <v-container v-if="price > 0">
        <v-form v-model="valid">
          <v-row>
            <v-col cols="12" md="12">
              <v-text-field
                v-model="price"
                label="Price (in yen)"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="4">
              <v-btn
                v-on:click="setCurrentPrice"
                block
                color="primary"
                elevation="3"
                small
                >Current</v-btn
              >
            </v-col>
            <v-col cols="12" md="4">
              <v-btn
                v-on:click="setFiftyTwoWeekHigh"
                block
                color="primary"
                elevation="3"
                small
                >Highest</v-btn
              >
            </v-col>
            <v-col cols="12" md="4">
              <v-btn
                v-on:click="setFiftyTwoWeekLow"
                block
                color="primary"
                elevation="3"
                small
                >Lowest</v-btn
              >
            </v-col>
            <v-col cols="12" md="12">
              <v-text-field
                v-model="volume"
                label="How much do you have? (Stock Volume)"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="12">
              <v-text-field
                v-model="taxrate"
                label="How much can you keep? (Taxrate in %)"
                required
              ></v-text-field>
            </v-col>
          </v-row>

          <v-btn
            v-on:click="calculate"
            block
            color="primary"
            elevation="3"
            large
            >Calculate</v-btn
          >
        </v-form>
        <Results :result="result"></Results>
      </v-container>
      <Loader v-else></Loader>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
import Loader from "./components/Loader.vue";
import Results from "./components/Results.vue";

export default {
  name: "App",
  components: {
    Results,
    Loader,
  },
  data: () => ({
    price: "",
    currentPrice: "",
    fiftyTwoWeekHigh: "",
    fiftyTwoWeekLow: "",
    taxrate: "42",
    exchangeRate: "",
    volume: "",
    result: "",
    valid: false,
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
  methods: {
    calculate() {
      let newtaxrate = Math.abs((this.taxrate - 100) * 0.01);
      this.result = new Intl.NumberFormat("de-DE", {
        style: "currency",
        currency: "EUR",
      }).format(((this.price * this.volume) / this.exchangeRate) * newtaxrate);
    },
    setFiftyTwoWeekLow() {
      this.price = this.fiftyTwoWeekLow;
    },
    setFiftyTwoWeekHigh() {
      this.price = this.fiftyTwoWeekHigh;
    },
    setCurrentPrice() {
      this.price = this.currentPrice;
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  margin-top: 80px;
  max-width: 375px;
}
</style>
