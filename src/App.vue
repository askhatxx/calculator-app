<template>
  <div id="app">
    <div class="tab-box">
      <ul class="tab-links">
        <li 
          class="tab-link"
          v-for='(tab, index) in tabs'
          :key='index'
          :class='{active: activeTab === index}'
          @click='activeTabChange(index)'
        >
            {{ tab }}
        </li>
      </ul>
      <div class="tab-content">
        <Calculator v-show='activeTab === 0'/>
        <Exchange v-show='activeTab === 1'/>
      </div>
    </div>
  </div>
</template>

<script>
import Calculator from './components/Calculator.vue';
import Exchange from './components/Exchange.vue';

export default {
  name: 'app',
  data() {
    return {
      tabs: ['Calculator', 'Exchange'],
      activeTab: 0
    }
  },
  components: {
    Calculator, Exchange
  },
  methods: {
    activeTabChange(index) {
      this.activeTab = index;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background: #f2f2f2;
  font-family: Arial, sans-serif;
}
#app {
  display: flex;
  justify-content: center;
  padding-top: 50px;
}
.tab-box {
  width: 90%;
  max-width: 370px;
}
.tab-links {
  list-style: none;
  display: flex;
}
.tab-links .tab-link {
  background: #fff;
  border: 1px solid #dfe1e5;
  border-bottom: none;
  border-radius: 8px 8px 0px 0px;
  padding: 10px 16px 9px;
  margin-right: 5px;
  color: #202124;
  font-size: 15px;
  cursor: pointer;
  user-select: none;
}
.tab-links .tab-link:last-child {
  margin-right: 0px;
}
.tab-links .tab-link.active {
  margin-bottom: -1px;
}
.tab-content {
  background: #fff;
  border: 1px solid #dfe1e5;
  border-radius: 0px 8px 8px 8px;
  padding: 16px;
}
</style>
