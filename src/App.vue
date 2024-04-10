

<template>
  <div class="card">
    <header><h2>Mortgage calculator</h2></header>
    <main>
      <div class="element">
        <label for="price">Purchase price: <strong>{{ formatter.format(price) }}</strong></label><br>
        <input type="range" class="slider" id="price" v-model="price" 
          min="0" max="1000000" step="10000" />
      </div>
      <div class="element">
        <label for="payment">Down payment: <strong>{{ formatter.format(payment) }}</strong></label><br>
        <input type="range" class="slider" id="payment" v-model="payment" 
          min="0" max="1000000" step="5000" />
      </div>
      <div class="element">
        <label for="year">Repayment time: <strong>{{ year }} years</strong></label><br>
        <input type="range" class="slider" id="year" v-model="year" 
          min="0" max="30" step="1" />
      </div>
      
      <div class="element">
        <label for="rate">Interest rate: <strong>{{ rate }}%</strong></label><br>
        <input type="range" class="slider" id="rate" v-model="rate" 
          min="0" max="10" step="0.1" />
      </div>
      <div class="element">
        Loan amount<br>
        <strong>{{ formatter.format(loan) }}</strong>
      </div>
      <div class="element">
        Estimated pr. month<br>
        <strong v-if="M == null">$-</strong>
        <strong v-else>{{ formatter.format(M) }}</strong>
      </div>
      
      <button @click="calculate()">Get a martage quote</button>
    </main>
  </div>
</template>

<style scoped>

</style>
<script setup>
import { computed, ref, watch } from 'vue';
const price = ref(450000);
const payment = ref(135000);
const year = ref(25);
const rate = ref(3);
const loan = computed(() => {
  return price.value - payment.value;
})
const M = ref();
watch(payment, (newVal) => {
  if (newVal > price.value) {
    payment.value = price.value;
  }
});
watch(price, (newVal) => {
  if (newVal < payment.value) {
    payment.value = newVal;
  }
});

function calculate() {
  const P = loan.value;
  const r = rate.value / 100 / 12;
  const n = year.value * 12;
  M.value = Math.round((P * r) * (Math.pow((1 + r), n)) / (Math.pow((1 + r), n) - 1));
}
const formatter = new Intl.NumberFormat('en-US', {
  style: 'currency',
  currency: 'USD',
  minimumFractionDigits: 0
});
</script>
