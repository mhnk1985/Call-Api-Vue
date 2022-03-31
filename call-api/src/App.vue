<template>
  <!--
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
  -->
  <div>
    
    <nav class="toolbar" role="banner">
      <span>{{ appName }}</span>
    </nav>

    <div class="container">
      <div class="output-container">
        
        <div class="chartName-output">
          updated: <strong>{{ updated }}</strong>
        </div> 

        <div class="chartName-output">
          ChartName: <strong>{{ chartName }}</strong>
        </div>
        <div class="usd-price-output">
          BTC: <strong>{{ usdValue }}</strong> <small> USD</small>
        </div>
        <div class="rial-price-output">
          BTC: <strong>{{ rialValue }}</strong> <small> Rial</small>
        </div>
      </div>
      <input
        type="number"
        class="usd-price-input"
        v-model="usdValueInput"
        placeholder="Enter USD Price To Rial ..."
      />
      <button class="btn-convert" @click="fetchPrice">
        Get Btc Price & Convert To Rial
      </button>
    </div>
    <div style="color: green">
      <small> RequestResult = </small> {{ RequestResult }}
    </div>
  </div>
</template>

<script>
/*
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
*/
export default {
  name: "App",
  data() {
    return {
      appName: "Vue Get Btc Price",
      rialValue: 0,
      usdValue: 0,
      usdValueInput: "",
      RequestResult: "",
      chartName: "",
      updated:""
    };
  },
  methods: {
    fetchPrice() {
      fetch("https://api.coindesk.com/v1/bpi/currentprice.json")
        .then((result) => {
          return result.json();
        })
        .then((data) => {
          this.RequestResult = data;

          try {
            this.updated = data.time.updated;
            this.chartName = data.chartName;

            let usdItem = data.bpi.USD;
            this.usdValue = usdItem.rate;
            this.rialValue =
              parseInt(this.usdValueInput | 0) * this.usdValue.replace(",", "");

            //this.rialValue = this.rialValue.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,');
            this.rialValue = this.rialValue.toLocaleString("en-US");
          } catch (err) {
            this.RequestResult = err;
          }
        })
        .catch((err) => {
          console.log("ssss");
          console.log(err);
        });
    },
  },
};
</script>

<style>
/*
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
*/ 
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica,
    Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  box-sizing: border-box;
}

.toolbar {
  position: sticky;
  top: 0;
  left: 0;
  right: 0;
  height: 60px;
  display: flex;
  align-items: center;
  background-color: #1976d2;
  color: white;
  font-weight: 600;
}

.container {
  margin: 120px 75px 0 75px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.rial-price-output {
  font-size: 45px;
}

.usd-price-output {
  font-size: 45px;
  margin-top: 20px;
}
.chartName-output {
  font-size: 45px;
}

.usd-price-input {
  height: 40px;
  width: 230px;
  margin-top: 50px;
  padding-left: 10px;
  padding-right: 10px;
  border: 2px solid rgba(100, 100, 100, 0.3);
  border-radius: 5px;
  outline: none;
  font-size: 16px;

  transition: 200ms;
}

.usd-price-input:hover {
  border: 2px solid rgba(117, 177, 226, 0.4);
}

.usd-price-input:focus {
  border: 2px solid rgba(0, 86, 156, 0.4);
}

.btn-convert {
  cursor: pointer;
  margin-top: 30px;
  width: 250px;
  height: 40px;
  border-radius: 5px;
  border: 0;
  outline: 0;
  font-size: 16px;
  color: white;
  background-color: #3ba5ec;

  transition: 200ms;
}

.btn-convert:hover {
  background-color: #2a8cce;
}

.btn-convert:active {
  background-color: #176da7;
}
</style>
