<template>
  <div class="exchange">
    <div v-if='apiError' class="api-error">Error</div>
    <div v-else-if='!apiResult' class="loading">Loading...</div>
    <div v-else>
      <div class="currency currency-one" :class='focusInput === 1 ? validateError : ""'>
        <div class="currency-input">
          <input v-model='currencyOne' @focus='focusInput = 1' @blur='validateError = ""'/>
        </div>
        <div class="currency-select">
          <select v-model='selctedOne'>
            <option 
              v-for='rate in rates'
              :key='rate'
              :value='rate'
            >
                {{ rate }} - {{ currencyName[rate] }}
            </option>
          </select>
        </div>
      </div>
      <div class="reverse">
        <button @click='currencyReverse' class="btn-reverse">↔</button>
      </div>
      <div class="currency currency-two" :class='focusInput === 2 ? validateError : ""'>
        <div class="currency-input">
          <input v-model='currencyTwo' @focus='focusInput = 2' @blur='validateError = ""'/>
        </div>
        <div class="currency-select">
          <select v-model='selctedTwo'>
            <option 
              v-for='rate in rates'
              :key='rate'
              :value='rate'
            >
                {{ rate }} - {{ currencyName[rate] }}
            </option>
          </select>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currencyOne: '0',
      currencyTwo: '0',
      selctedOne: 'USD',
      selctedTwo: 'RUB',
      apiResult: null,
      apiError: false,
      focusInput: null,
      validateError: '',
      currencyName: currencyName
    }
  },
  created() {
    const api = 'https://api.exchangerate-api.com/v4/latest/USD';
    const apiReserve = 'https://api.exchangeratesapi.io/latest';
    fetch(api)
      .then(response => response.json())
      .then(result => this.apiResult = result)
      .catch(() => {
        fetch(apiReserve)
          .then(response => response.json())
          .then(result => this.apiResult = result)
          .catch(() => this.apiError = true)
      });
  },
  watch: {
    currencyOne() {
      this.currencyOne = this.validate(this.currencyOne);
      if (this.focusInput === 1) this.calculateOne();
    },
    currencyTwo() {
      this.currencyTwo = this.validate(this.currencyTwo);
      if (this.focusInput === 2) this.calculateTwo();
    },
    selctedOne() {
      this.calculateOne();
    },
    selctedTwo() {
      this.calculateOne();
    }
  },
  computed: {
    rates() {
      if (!this.apiResult) return [];
      return Object.keys(this.apiResult.rates).sort();
    }
  },
  methods: {
    calculateOne() {
      const inBase = Number(this.currencyOne) / this.apiResult.rates[this.selctedOne];
      this.currencyTwo = (inBase * this.apiResult.rates[this.selctedTwo]).toFixed(2);
    },
    calculateTwo() {
      const inBase = Number(this.currencyTwo) / this.apiResult.rates[this.selctedTwo];
      this.currencyOne = (inBase * this.apiResult.rates[this.selctedOne]).toFixed(2);
    },
    validate(str) {
      return [...str].reduce((result, char) => {
        if ('0123456789'.includes(char)) return `${result}${char}`;
        if ('.,'.includes(char) && !result.includes('.')) return `${result}.`;
        this.validateError = this.validateError === '' ? 'err1' : 
          this.validateError === 'err1' ? 'err2' : 'err1';
        return result;
      }, '');
    },
    currencyReverse() {
      [this.selctedOne, this.selctedTwo] = [this.selctedTwo, this.selctedOne];
    }
  }
}

const currencyName = {
"AED":"UAE Dirham",
"ARS":"Argentine Peso",
"AUD":"Australian Dollar",
"BGN":"Bulgarian Lev",
"BRL":"Brazilian Real",
"BSD":"Bahamian Dollar",
"CAD":"Canadian Dollar",
"CHF":"Swiss Franc",
"CLP":"Chilean Peso",
"CNY":"Chinese Renminbi",
"COP":"Colombian Peso",
"CZK":"Czech Koruna",
"DKK":"Danish Krone",
"DOP":"Dominican Peso",
"EGP":"Egyptian Pound",
"EUR":"Euro",
"FJD":"Fiji Dollar",
"GBP":"Pound Sterling",
"GTQ":"Guatemalan Quetzal",
"HKD":"Hong Kong Dollar",
"HRK":"Croatian Kuna",
"HUF":"Hungarian Forint",
"IDR":"Indonesian Rupiah",
"ILS":"Israeli Shekel",
"INR":"Indian Rupee",
"ISK":"Icelandic Krona",
"JPY":"Japanese Yen",
"KRW":"South Korean Won",
"KZT":"Kazakhstani Tenge",
"MXN":"Mexican Peso",
"MYR":"Malaysian Ringgit",
"NOK":"Norwegian Krone",
"NZD":"New Zealand Dollar",
"PAB":"Panamanian Balboa",
"PEN":"Peruvian Nuevo Sol",
"PHP":"Philippine Peso",
"PKR":"Pakistani Rupee",
"PLN":"Polish Zloty",
"PYG":"Paraguayan Guarani",
"RON":"Romanian Leu",
"RUB":"Russian Ruble",
"SAR":"Saudi Riyal",
"SEK":"Swedish Krona",
"SGD":"Singapore Dollar",
"THB":"Thai Baht",
"TRY":"Turkish Lira",
"TWD":"New Taiwan Dollar",
"UAH":"Ukrainian Hryvnia",
"USD":"US Dollar",
"UYU":"Uruguayan Peso",
"ZAR":"South African Rand"
};
</script>

<style scoped>
.exchange {
  margin: 20px 0px;
}
.currency {
  display: flex;
  border: 1px solid #ebebeb;
  border-radius: 4px;
}
.currency-input {
  flex-grow: 1;
}
.currency-select {
  width: 120px;
}
.currency input {
  width: 100%;
  height: 100%;
  font-size: 20px;
  font-family: inherit;
  padding: 8px;
  border: none;
  background: none;
  outline: none;
}
.currency select {
  width: 100%;
  height: 100%;
  font-size: 14px;
  font-family: inherit;
  padding-left: 4px;
  border: none;
  border-left: 1px solid #ebebeb;
  background: none;
  outline: none;
}
.reverse {
  text-align: center;
  margin: 12px 0px;
}
.btn-reverse {
  background: #dfe1e5;
  border: 1px solid #dfe1e5;
  border-radius: 4px;
  font-family: inherit;
  color: #202124;
  line-height: 34px;
  font-size: 14px;
  text-align: center;
  cursor: pointer;
  outline: none;
  user-select: none;
  width: 78px;
}
.btn-reverse:hover {
  background: #d7d9dd;
  border-color: #d7d9dd;
}
.btn-reverse:active {
  background: #d7d9dd;
  border-color: #9aa0a6;
}
.api-error {
  background: rgba(255, 0, 0, .25);
  padding: 10px;
  border-radius: 4px;
  font-size: 18px;
}
.loading {
  background: #dfe1e5;
  padding: 10px;
  border-radius: 4px;
  font-size: 18px;
}
.err1 {
  animation-duration: 1s;
  animation-name: erroranim1;
}
.err2 {
  animation-duration: 1s;
  animation-name: erroranim2;
}
@keyframes erroranim1 {
  0% {
    background: rgba(255, 0, 0, .25);
  }
  100% {
    background: rgba(255, 0, 0, .0);
  }
}
@keyframes erroranim2 {
  0% {
    background: rgba(255, 0, 0, .25);
  }
  100% {
    background: rgba(255, 0, 0, .0);
  }
}
</style>