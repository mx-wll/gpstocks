<template>
  <v-container>
    <v-form v-model="valid">
      <v-row>
        <v-col cols="12" md="12">
          <v-text-field
            v-model="price"
            :rules="numberRules"
            label="Price"
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
            >currentPrice</v-btn
          >
        </v-col>
        <v-col cols="12" md="4">
          <v-btn
            v-on:click="setFiftyTwoWeekHigh"
            block
            color="primary"
            elevation="3"
            small
            >fiftyTwoWeekHigh</v-btn
          >
        </v-col>
        <v-col cols="12" md="4">
          <v-btn
            v-on:click="setFiftyTwoWeekLow"
            block
            color="primary"
            elevation="3"
            small
            >fiftyTwoWeekLow</v-btn
          >
        </v-col>
        <v-col cols="12" md="12">
          <v-text-field
            v-model="volume"
            :rules="numberRules"
            label="Volume"
            required
          ></v-text-field>
        </v-col>

        <v-col cols="12" md="12">
          <v-text-field
            v-model="taxrate"
            :rules="numberRules"
            label="Taxrate"
            required
          ></v-text-field>
        </v-col>
      </v-row>

      <v-btn v-on:click="calculate" block color="primary" elevation="3" large
        >Calculate</v-btn
      >
    </v-form>
    <p>{{ result }}</p>
  </v-container>
</template>

<script>
export default {
  name: "Calculator",
  props: [
    "price",
    "currentPrice",
    "fiftyTwoWeekHigh",
    "fiftyTwoWeekLow",
    "taxrate",
    "exchangeRate",
  ],
  data: () => ({
    volume: "",
    result: "",
    valid: false,
    numberRules: [(v) => !!v || "Is required"],
  }),
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
