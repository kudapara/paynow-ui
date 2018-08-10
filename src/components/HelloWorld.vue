<template>
  <div class="hello">
    <h1>Pay Your Subs</h1>
    <input type="number" placeholder="Amount" v-model="payment.amount">


    <button @click="payNow">Pay now</button>

    <ul>
      <li v-for="tx in transactions" :key="tx._id">
        ${{tx.amount}} {{tx.status}} <button>See more</button>
      </li>
    </ul>
  </div>
</template>

<script>
import qs from 'querystring'
import axios from 'axios'

export default {
  name: 'HelloWorld',
  mounted () {
    // retrieve the poll url of the last transaction and check payment
    this.getAllTransactions()
  },
  data () {
    return {
      payment: {
        amount: 20,
        authemail: 'kgparadzayi@gmail.com'
      },
      transactions: []
    }
  },
  methods: {
    payNow () {
      axios.post('https://paynow.now.sh/pay', this.payment)
        .then(response => {
          location = response.data.browserurl
        })
        .catch(console.log)
    },

    getAllTransactions () {
      axios.get('https://paynow.now.sh')
        .then(response => {
          this.transactions = response.data.data
        })
        .catch(console.log)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 20px;
}

a {
  color: #42b983;
}
</style>
