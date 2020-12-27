<template>
  <div class="container-app">
    <div class="currency-container">
      <div class="currency-header">
        <h1>Currency Converter</h1>
        <h4>Last Update: {{ date }}</h4>
      </div>
      <div class="currency-main">
        <div class="row">
          <div class="col-md-12 currency">
            <input class="form-control" type="number" v-model="amount" />
          </div>
          <div class="col-5">
            <div class="form-group">
              <label>From</label>
              <select class="form-control" v-model="from">
                <option v-for="rate in rates" :key="rate.id">
                  {{ rate }}
                </option>
              </select>
            </div>
          </div>
          <div class="col-2 swap">
            <i @click="swap" class="fas fa-exchange-alt"></i>
          </div>
          <div class="col-5">
            <div class="form-group">
              <label>To</label>
              <select class="form-control" v-model="to">
                <option v-for="rate in rates" :key="rate.id">
                  {{ rate }}
                </option>
              </select>
            </div>
          </div>
          <div class="col-12 converter">
            <button @click="converter" class="btn btn-light">Converter</button>
          </div>
          <div class="col-12 currency-result" v-if="clicked">
            <h2><i class="fas fa-coins"></i> {{ result }} {{ to }}</h2>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      rates: [],
      to: "",
      from: "",
      amount: 0,
      date: "",
      result: 0,
      clicked: false,
    };
  },
  created() {
    axios
      .get("https://api.exchangeratesapi.io/latest?base=AUD")
      .then((result) => {
        this.date = result.data.date;
        var newRates = result.data.rates;
        for (let [key, value] of Object.entries(newRates)) {
          this.rates.push(key);
          this.rates.sort();
        }
      });
  },
  methods: {
    converter() {
      this.clicked = true;
      axios
        .get("https://api.exchangeratesapi.io/latest?base=" + this.from)
        .then((result) => {
          var data = result.data.rates;
          this.result = (data[this.to] * this.amount).toFixed(2);
        });
    },
    swap() {
      this.clicked = false;
      var from = this.from;
      this.from = this.to;
      this.to = from;
    },
  },
};
</script>

<style lang="scss" scoped>
* {
  font-family: "Poppins", sans-serif;
  margin: 0;
  letter-spacing: 2px;
}
.container-app {
  display: flex;
  width: 100%;
  height: 100%;
  justify-content: center;

  .currency-container {
    display: flex;
    width: 70%;
    height: 70%;
    position: absolute;
    top: 0;
    bottom: 0;
    margin: auto;
    flex-flow: column;
    text-align: center;
    border-radius: 0px 50px 0px 50px;
    box-shadow: 0 10px 20px black;
    background-color: rgb(122, 122, 3);
    animation: fadeUp 4s ease;

    .currency-header {
      display: flex;
      flex-flow: column;
      width: 100%;
      height: 30%;
      justify-content: center;
      align-items: center;
      border-bottom: 1px solid black;
      border-radius: 0px 50px 0px 0px;

      & h4 {
        font-size: 0.8rem;
        font-style: italic;
        font-weight: bold;
        margin-top: 10px;
        color: black;
      }
    }

    .currency-main {
      display: flex;
      justify-content: center;
      margin-top: 20px;
      width: 100%;
      height: 70%;

      .currency {
        margin-bottom: 20px;
      }

      .swap {
        display: flex;
        flex-flow: column;
        margin-top: 30px;
        cursor: pointer;
        & i {
          font-size: 1.5rem;
          color: black;
        }
      }
    }

    .converter {
      display: flex;
      flex-flow: row;
      justify-content: center;
      width: 100%;
      height: 45%;
      margin-top: 20px;

      & button {
        width: 150px;
        height: 35px;
        outline: none;
        border: 0;
        margin: auto;
        margin-top: 20px;
        color: white;
        font-size: 18px;
        border-radius: 15px;
        background-color: #000000f5;
      }
    }
    .currency-result {
      display: flex;
      flex-flow: row;
      justify-content: center;
      margin: auto;
      position: relative;
      top: -60px;
      width: 100%;
      height: 20%;

      h2 {
        font-weight: bold;
      }
    }
  }
}

@keyframes fadeUp {
  from {
    transform: translateY(-100px);
  }
  to {
    transform: translateY(0);
  }
}
@media screen and (max-width: 420px) {
  .currency-header {
    & h1 {
      font-size: 1.5rem;
    }
  }
}
</style>