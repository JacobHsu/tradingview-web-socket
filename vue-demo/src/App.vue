<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import { apiGet } from "./api";
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data() {
    return {
      symbol: "",
      symbolInfo: {},
      symbolList: [],
    };
  },
  methods: {
    fetchSymbolList() {
      apiGet("common_symbols").then((res) => {
        if (!res || !res.data) {
          return;
        }
        const list = [];
        for (let i = 0; i < res.data.length; i++) {
          if (res.data[i]["quote-currency"] === "usdt") {
            list.push(res.data[i]);
          }
        }
        const symbol = list.length ? list[0].symbol : "";
        this.symbol = symbol;
        this.symbolInfo = list[0];
        this.symbolList = list;
        console.log(symbol, list[0]) // dfusdt {...}
      });
    },
  },
  created() {
    this.fetchSymbolList();
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
