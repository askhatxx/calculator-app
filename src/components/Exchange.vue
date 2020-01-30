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
              {{ rate }}
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
              {{ rate }}
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
      selctedOne: 'RUB',
      selctedTwo: 'KZT',
      apiResult: null,
      apiError: false,
      focusInput: null
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
      const currencyOne = this.validate(this.currencyOne);
      const inBase = Number(currencyOne) / this.apiResult.rates[this.selctedOne];
      const currencyTwo = (inBase * this.apiResult.rates[this.selctedTwo]).toFixed(2);
      this.currencyOne = currencyOne;
      if (this.focusInput === 1) this.currencyTwo = currencyTwo;
    },
    currencyTwo() {
      const currencyTwo = this.validate(this.currencyTwo);
      const inBase = Number(currencyTwo) / this.apiResult.rates[this.selctedTwo];
      const currencyOne = (inBase * this.apiResult.rates[this.selctedOne]).toFixed(2);
      this.currencyTwo = currencyTwo;
      if (this.focusInput === 2) this.currencyOne = currencyOne;
    }
  },
  computed: {
    rates() {
      if (!this.apiResult) return [];
      return Object.keys(this.apiResult.rates).sort();
    }
  },
  methods: {
    validate(str) {
      return [...str].reduce((result, char) => {
        if ('0123456789'.includes(char)) return `${result}${char}`;
        if ('.,'.includes(char) && !result.includes('.')) return `${result}.`;
        return result;
      }, '');
    },
    currencyReverse() {
      [this.selctedOne, this.selctedTwo] = [this.selctedTwo, this.selctedOne]
    }
  }
}
</script>