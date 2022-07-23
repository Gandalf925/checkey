<template>
  <div>
    <h1 class="d-flex justify-center mb-6">Generate BSV PrivateKeys</h1>
    <v-btn color="primary" @click="generateKeys()">Generate Key</v-btn>
    <v-row v-for="(privKey, index) in privKeys" :key="index">
      <v-col xs="6">
        <v-card>
          <v-card-text>PrivateKey{{ index + 1 }} = {{ privKey }}</v-card-text>
          <v-card-text
            >PublicKey{{ index + 1 }} = {{ pubKeys[index] }}</v-card-text
          >
          <v-card-text
            >Address{{ index + 1 }} = {{ addresses[index] }}</v-card-text
          >
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script lang="js">
import * as bsv from 'bsv'
export default {
  data() {
    return {
      privKey: '',
      pubKey: '',
      address: '',
      privKeys: [],
      pubKeys: [],
      addresses: [],
    }
  },
  mounted() {
    if (localStorage.getItem('privKeys')) {
      try {
        this.privKeys = JSON.parse(localStorage.getItem('privKeys'))
        this.pubKeys = JSON.parse(localStorage.getItem('pubKeys'))
        this.addresses = JSON.parse(localStorage.getItem('addresses'))
      } catch(e) {
        localStorage.removeItem('privKeys')
        localStorage.removeItem('pubKeys')
        localStorage.removeItem('addresses')
      }
    }
  },
  methods: {
    generateKeys(){
      this.privKey = bsv.PrivateKey.fromRandom()
      this.pubKey = bsv.PublicKey.fromPrivateKey(this.privKey)
      this.address = bsv.Address.fromPublicKey(this.pubKey)

      this.privKeys.push(this.privKey.toString())
      this.pubKeys.push(this.pubKey.toString())
      this.addresses.push(this.address.toString())
      this.saveKeys();
    },
    saveKeys() {
      const parsedPrivKeys = JSON.stringify(this.privKeys)
      const parsedPubKeys = JSON.stringify(this.pubKeys)
      const parsedAddresses = JSON.stringify(this.addresses)
      localStorage.setItem('privKeys', parsedPrivKeys)
      localStorage.setItem('pubKeys', parsedPubKeys)
      localStorage.setItem('addresses', parsedAddresses)
    }
  }
}
</script>

<style></style>
