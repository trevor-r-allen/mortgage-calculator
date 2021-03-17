<template>
  <div class="home flex-grow-1 d-flex flex-column align-items-center justify-content-center">
    <form>
      <div class="form-group">
        <label for="amount">Amount</label>
        <input v-model="state.amount" type="number" class="form-control" id="amount">
      </div>
      <div class="form-group">
        <label for="interest-rate">Interest Rate</label>
        <input v-model="state.interestRate" type="number" step="any" class="form-control" id="interest-rate">
      </div>
      <label for="term">Term</label>
      <select v-model="state.term" class="custom-select">
        <option value="15" selected>
          15
        </option>
        <option value="30">
          30
        </option>
      </select>
      <button @click="calculateMortgage(state.amount, state.interestRate, state.term)" type="submit" class="btn btn-primary">
        Submit
      </button>
    </form>
    <p><strong>Total Balance</strong> {{ state.balance }}</p>
    <p><strong>Monthly Payment</strong> {{ state.payment }}</p>
  </div>
</template>

<script>
import { reactive } from 'vue'
export default {
  name: 'Home',
  setup() {
    const state = reactive({
      amount: 100000,
      interestRate: 5.5,
      term: 15,
      balance: 0,
      payment: 0
    })
    return {
      state,
      calculateMortgage(amount, interest, term) {
        if (!amount || !interest || !term) return { balance: 0, pmt: 0 }

        const r = (interest * 0.01) / 12
        const n = term * 12
        const p = amount

        const balance = ((r * p * n) / (1 - Math.pow(1 + r, -n))).toFixed(2)
        const pmt = (balance / n).toFixed(2)

        console.log({ amount, interest, term })
        console.log({ balance, pmt })

        state.balance = balance
        state.payment = pmt
      }
    }
  }
}
</script>

<style scoped lang="scss">
.home{
  text-align: center;
  user-select: none;
  > img{
    height: 200px;
    width: 200px;
  }
}
</style>
