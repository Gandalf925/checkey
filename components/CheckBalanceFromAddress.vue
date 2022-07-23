<template>
  <div>
    <h1 class="d-flex justify-center">Check Balance</h1>
    <v-text-field v-model="address" label="input Address"></v-text-field>
    <v-btn color="primary" @click="checkBalanceFromAddress()">Check</v-btn>
    <p v-show="confirmedBalance !== ''" class="mt-5">
      {{ confirmedBalance }} BSV (confirmed)
    </p>
    <p v-show="unconfirmedBalance !== ''">
      {{ unconfirmedBalance }} BSV (unconfirmed)
    </p>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      address: '',
      confirmedBalance: '',
      unconfirmedBalance: '',
    }
  },
  methods: {
    // eslint-disable-next-line vue/return-in-computed-property
    checkBalanceFromAddress() {
      if (this.address !== '') {
        // eslint-disable-next-line vue/no-async-in-computed-properties
        axios
          .get(
            `https://api.whatsonchain.com/v1/bsv/main/address/${this.address}/balance`
          )
          .then((res) => {
            this.confirmedBalance = res.data.confirmed
            this.unconfirmedBalance = res.data.unconfirmed
          })
          .catch((err) => {
            console.log('err:', err)
          })
      }
    },
  },
}
</script>

<style></style>
