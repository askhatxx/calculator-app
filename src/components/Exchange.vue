<template>
  <div class="exchange">
    <div class="currency-one">
      <input v-model='currencyOne'/>
      <select v-model='selctedOne'>
        <option value='a'>Item A</option>
        <option value='b'>Item B</option>
        <option value='c'>Item C</option>
      </select>
    </div>
    <div class="currency-two">
      <input v-model='currencyTwo'/>
      <select v-model='selctedTwo'>
        <option value='a'>Item A</option>
        <option value='b'>Item B</option>
        <option value='c'>Item C</option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currencyOne: '0',
      currencyTwo: '0',
      selctedOne: 'a',
      selctedTwo: 'b'
    }
  },
  watch: {
    currencyOne() {
      this.currencyOne = this.validate(this.currencyOne);
    },
    currencyTwo() {
      this.currencyTwo = this.validate(this.currencyTwo);
    }
  },
  methods: {
    validate(str) {
      return [...str].reduce((result, char) => {
        if ('0123456789'.includes(char)) return `${result}${char}`;
        if ('.,'.includes(char) && !result.includes('.')) return `${result}.`;
        return result;
      }, '');
    }
  }
}
</script>