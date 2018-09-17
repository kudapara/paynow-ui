<template>
  <div id="app">
    <!--<nav class="border-b bg-white">
      <div class="container mx-auto flex justify-between">
        <router-link to="/" class="font-bold no-underline text-grey-darkest  py-4">KayPay</router-link>
        <div class="flex">
          <router-link to="/coordinates" class="block  py-4 px-6 border-b-2 border-transparent no-underline hover:border-purple mr-4no-underline text-grey-darkest">Coordinates</router-link>
          <router-link to="/transactions" class="block  py-4 px-6 border-b-2 border-transparent no-underline hover:border-purple mr-4no-underline text-grey-darkest">Transactions</router-link>
        </div>
      </div>
    </nav>-->
    <router-view/> <!-- class="container mx-auto"/> -->
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
  @import './assets/app.css';
  body {
    font-family: 'Roboto', sans-serif;
  }
</style>
