<template>
  <div class="exchange">
    <div v-if='apiError'>Error</div>
    <div v-else-if='!apiResult'>Loading...</div>
    <div v-else>
      <div class="currency-one">
        <input v-model='currencyOne' @focus='focusInput = 1'/>
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
      <div @click='currencyReverse'>x</div>
      <div class="currency-two">
        <input v-model='currencyTwo' @focus='focusInput = 2'/>
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
</template>

<script>
export default {
  data() {
    return {
      currencyOne: '0',
      currencyTwo: '0',
      selctedOne: 'USD',
      selctedTwo: 'KZT',
      apiResult: null,
      apiError: false,
      focusInput: null,
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