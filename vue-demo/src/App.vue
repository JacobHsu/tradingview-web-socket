<template>
  <div id="app">
    <div class="box">
      <div class="left-box">
        <KLineHeader
          :symbol="symbol"
          :symbolList="symbolList"
          @onClick="onClick"
        />
      </div>
      <div class="right-box">
        <KLineWidget
          v-if="!!symbolInfo.symbol"
          :symbolInfo="symbolInfo"
          ref="kLineWidget"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { apiGet } from "./api";
import { ws } from "./utils/socket";
import KLineHeader from "./components/KLineHeader";
import KLineWidget from "./components/KLineWidget";

export default {
  name: 'App',
  components: {
    KLineHeader,
    KLineWidget,
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
        // console.log(symbol, list) // dfusdt {...}
      });
    },
    onClick(item) {
      this.$refs.kLineWidget.setSymbol(item.symbol);
    },
  },
  created() {
    ws.initWebSocket();
    this.fetchSymbolList();
  },
}
</script>

<style>
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
    "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.box {
  display: flex;
}
.left-box {
  width: 160px;
}
.right-box {
  margin-left: 10px;
  width: auto;
  flex-grow: 1;
}
.header {
  overflow: scroll;
}
.secondary {
  line-height: 1.5715;
  position: relative;
  display: inline-block;
  font-weight: 400;
  white-space: nowrap;
  text-align: center;
  background-image: none;
  box-shadow: 0 2px 0 rgba(0, 0, 0, 0.015);
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
  touch-action: manipulation;
  height: 32px;
  padding: 4px 15px;
  font-size: 14px;
  border-radius: 2px;
  color: #ff4d4f;
  background: #fff;
  border-color: #ff4d4f;
  margin: 10px;
}
.primary {
  line-height: 1.5715;
  position: relative;
  display: inline-block;
  font-weight: 400;
  white-space: nowrap;
  text-align: center;
  background-image: none;
  box-shadow: 0 2px 0 rgba(0, 0, 0, 0.015);
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
  touch-action: manipulation;
  height: 32px;
  padding: 4px 15px;
  font-size: 14px;
  border-radius: 2px;
  background: #fff;
  color: rgba(0, 0, 0, 0.85);
  border: 1px solid #d9d9d9;
  margin: 10px;
}
</style>
