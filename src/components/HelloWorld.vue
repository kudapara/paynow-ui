<template>
  <div class="hello">
    <div class="flex justify-end mt-4 mb-4">
      <div>
        <input type="number" placeholder="Enter the amount to pay" v-model="payment.amount" class="bg-white py-2 px-4 border rounded mr-2">
        <button @click="payNow" class="bg-purple text-white rounded py-2 px-4">Pay now</button>
      </div>
    </div>

    <div class="flex items-center justify-between bg-white p-4 rounded-t shadow">
      <div class="flex items-center">
        <h3 class="pr-2 mr-4 border-r-2">Transactions</h3>
        <input type="text" placeholder="search transactions">
      </div>

      <div class="flex items-center">
        <select name="" id="" class="border-2 border-grey rounded p-1 mr-4">
          <option value="s">filter</option>
        </select>
        <button class="text-purple  py-2 px-6 rounded border-2 border-purple">Download CSV</button>
      </div>
    </div>
    <div class="flex justify-between p-4 bg-grey-dark text-white shadow">
      <span> Email Address </span>
      <span> Product </span>
      <span> Amount </span>
      <span> Date </span>
    </div>
    <table class="w-full bg-white px-4 shadow">
      <tbody>
        <tr v-for="(tx, i) in transactions" :key="tx._id">
          <td class="pl-4 py-2 border-b border-grey-light text-sm"> <span class="border-2 rounded-full inline-block py-1 px-4 mr-2" :class="statusStyle(tx.status)" v-if="tx.status.toUpperCase() !== 'PAID'">{{ tx.status }}</span> {{ tx.reference.split('=')[1] || 'not available'}} </td>
          <td class="py-2 border-b border-grey-light text-sm"> {{ tx.reference.split('=')[0] }} </td>
          <td class="py-2 border-b border-grey-light text-sm"> ${{ tx.amount }} </td>
          <td class="py-2 border-b border-grey-light text-sm"> {{ tx.timestamp }} </td>
          <td class="py-2 border-b border-grey-light text-sm pr-2 border-b border-grey-light text-sm text-right">
            <button class="py-2 px-4 border-2 border-grey rounded nowrap">more</button>
          </td>
        </tr>
      </tbody>
    </table>
   

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
      transactions: [{"_id":"5b68a627b69d5500549eca24","reference":"hitconnect=kgparadzayi@gmail.com","paynowreference":"1422956","amount":"20.00","status":"Paid","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=488b8180-b676-47b0-8fe1-8bdf7aac2bf4","hash":"32BB4C8C19600969C24DDDC56F075754295F56CE373D858612474661D2FD439754F1C8CCA3F9725B7716109B166B59F918BF480A80D98215C8759EEFA09EE0EA","timestamp":"2018-08-06T19:48:55.405Z","__v":0},{"_id":"5b68a816b69d5500549eca27","reference":"hitconnect=kgparadzayi@gmail.com","paynowreference":"1422986","amount":"45.00","status":"Paid","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=eb855a63-7207-44ba-bde1-864664033429","hash":"42D6FF88AEF10F3F1A0F20A3627852D9C67D42C494E1EBB2FABC22A101B985D73446CE96A5A3737F0F4E45465093125CD90EED8E7944162E7C513D96A8DAF32E","timestamp":"2018-08-06T19:57:10.865Z","__v":0},{"_id":"5b693ec920ca8a004ed77e56","reference":"hitconnect=kgparadzayi@gmail.com","paynowreference":"1423536","amount":"100.00","status":"Awaiting Delivery","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=6633a3db-0c82-4133-8e06-8301d3c7c70a","hash":"2A0D4CCFF20FE5FE24035EF1C80E8DCC323161B4B307FC04CEC3CEA4C1AC24EEFA169387B31938F1A19C4085B88AD0AAE3A565F8E2B4A19C3CC1CFB2785DDB0C","timestamp":"2018-08-07T06:40:09.669Z","__v":0},{"_id":"5b6945ed20ca8a004ed77e5b","reference":"hitconnect=kgparadzayi@gmail.com","paynowreference":"1423687","amount":"51.00","status":"Awaiting Delivery","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=6f34f1a1-9eb9-4bf4-8bb3-013f05021924","hash":"FABF326606A0F64468DADE5D5ECE1AEF6D3B7E330E3CC50B32EF255F1B5FF015C92B4869350296062186152B61256B3A3FD0ED6B1362F1D20CD8AF71B91742DF","timestamp":"2018-08-07T07:10:37.061Z","__v":0},{"_id":"5b69d1e8cde312004fa6db84","reference":"232323","paynowreference":"1421401","amount":"1.00","status":"Cancelled","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=b0d74079-b320-4659-9e0d-2470e6c85200","hash":"C1FD3FD9BD1A1079ACD3A68633E7F0666C9A9AD7288EF2F2179F33A0C7F563C39363D3EF1D5B881AF8AF1FBB83302E7253316E07E578590707C1C9ECB7DD68D2","timestamp":"2018-08-07T17:07:52.691Z","__v":0},{"_id":"5b69d1e9cde312004fa6db85","reference":"232323","paynowreference":"1421402","amount":"1.00","status":"Cancelled","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=789fb111-44b3-48a1-97f1-313539b100ba","hash":"6CB5BBAAA05E4F1F7117AE4C1705A0F3AED79912558D2863066BBB152A440536B6574E286557E92758070EC42780F172AA6FBFE9C481413687325F2380210B57","timestamp":"2018-08-07T17:07:53.161Z","__v":0},{"_id":"5b6a8b5db4c90a0052c38eb6","reference":"hitconnect=kgparadzayi@gmail.com","paynowreference":"1427625","amount":"13.00","status":"Paid","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=f50acf6d-593b-483a-b049-8fcdc8e1e072","hash":"552758D29F439A80BFA3B81D3A5E88A00EEBAEF3269EE9BA50BE42CE825AD3E712FE9ED0725023F774278391E371B5A5F938EED0C6E8486CD0A1F97B05B36E36","timestamp":"2018-08-08T06:19:09.238Z","__v":0},{"_id":"5b6a8d19b4c90a0052c38eb8","reference":"hitconnect=kgparadzayi@gmail.com","paynowreference":"1427659","amount":"11.00","status":"Paid","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=3fefdd0b-6434-40b6-bbf4-1adb19f1f5f1","hash":"974E233C6B56E4DB076587827CBCBA9E2FF01D757C17CAD4818E1E12D3C1E8A9E0D78893A6E1DE1577A2DC82342ACE62911490FBF71E6004275D6BDED084F4E0","timestamp":"2018-08-08T06:26:33.389Z","__v":0},{"_id":"5b6a95a5b4c90a0052c38eba","reference":"hitconnect=kgparadzayi@gmail.com","paynowreference":"1427821","amount":"12.00","status":"Paid","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=c80e2a81-1625-4246-a3da-94ff53cdd4b4","hash":"C9F1D6D53C12FF4C193C9E93D70C247D45AE23A3038D05752558AC48F9F6D7A353D05F9E2640B1E0013E91130CD13EAB33746F0D382A5F8D9AACCDAF3D2272BF","timestamp":"2018-08-08T07:03:01.655Z","__v":0},{"_id":"5b6a96b29a5c03004d771dbe","reference":"hitconnect=kgparadzayi@gmail.com","paynowreference":"1427848","amount":"6.00","status":"Paid","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=920b6060-128b-4793-b520-918c6dcd2131","hash":"308197DC3BC3BD79CB213DD7C3E4CC40B4034F9780A723331AB9EE367D8EC7E852F8FE9467EACBB493847AFB3E96C3A1D5D94EB0402BC832F5580ABD1F82A99E","timestamp":"2018-08-08T07:07:30.117Z","__v":0},{"_id":"5b6b21d27a1b7a004ee01271","reference":"hitconnect=kgparadzayi@gmail.com","paynowreference":"1422948","amount":"50.00","status":"Cancelled","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=d0262136-fce7-468a-aa9a-f30ffc6fe4da","hash":"79A767C2AC5403741348117B99F53073A3128469B14DA18697866E22D39435DA5BAFC66DBB9D1DD920DDF5EAB4145317809B4B4B028AFDC7ADF8EF30A787D551","timestamp":"2018-08-08T17:01:06.550Z","__v":0},{"_id":"5b6b21d47a1b7a004ee01272","reference":"hitconnect=kgparadzayi@gmail.com","paynowreference":"1422952","amount":"20.00","status":"Cancelled","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=43bd5534-3363-4a2e-bace-42987754e230","hash":"A366342A7CC7A2CF7B8EBA6D8B7026B8D4603FCBEE2F534AA540532D1E53C7F05A569BA9BF152062C36FE6ED8906551F5690DF87E24EEBF1A8181126510732A6","timestamp":"2018-08-08T17:01:08.901Z","__v":0},{"_id":"5b6b21d87a1b7a004ee01273","reference":"hitconnect=kgparadzayi@gmail.com","paynowreference":"1422985","amount":"20.00","status":"Cancelled","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=daf195b9-a5a9-4f2b-b338-f8d426848166","hash":"C83E167BD804A5E8C6E21D0EED38A8ED9209EA14D79C65C3119C79DF6CC72C53F107B4CAC1EE82B1FA8115D323614755EE9C2D6CC0CD9FD9C22E93758D2F5E0B","timestamp":"2018-08-08T17:01:12.514Z","__v":0},{"_id":"5b6d8f76f82869004e902fc5","reference":"hitconnect=kgparadzayi@gmail.com","paynowreference":"1438743","amount":"5.00","status":"Paid","pollurl":"https://www.paynow.co.zw/Interface/CheckPayment/?guid=e0780459-8a28-47eb-9e3f-d782eb48e222","hash":"01DEA2A46025E3AFC924FD4662B6F07C5A93134EBB0710AEEABC7BBF20E97CA84AF0F837B842CBD39628AF3DF9A6027B3EBEEF09B6D6F4FEEF634657A99CD758","timestamp":"2018-08-10T13:13:26.623Z","__v":0}]
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
    },
    statusStyle (status) {
      console.log('Hello world, status')
      console.log(status)
      if (status.toUpperCase() === 'CANCELLED') {
        return {
          'border-red': true,
          'text-red': true
        }
      } else if (status.toUpperCase() === 'AWAITING DELIVERY') {
        return {
          'border-orange': true,
          'text-orange': true
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .bg-grey-superlight {
    background: rgba(18, 41, 32, 0.02)
  }
</style>
