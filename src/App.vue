<template>
  <div id="app">
    <nav class="nav">
      <router-link class="brand">KayPay</router-link>
      <div>
        <router-link to="/transactions">Transactions</router-link>
      </div>
    </nav>
    <router-view class="container"/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
  body {
    font-family: 'Roboto', sans-serif;
  }

  .container, .nav {
    width: 90%;
    margin: 0 auto
  }

  .nav {
    display: flex;
    justify-content: space-between;
    padding: 10px 0;
    border-bottom: rgba(0, 0, 0, 0.03) solid 2px;
  }

  .brand {
    font-weight: 900;
    font-size: 20px;
    /* letter-spacing: 5px; */
  }

  a {
    text-decoration: none;
    color: currentColor;
    display: inline-block;
    padding: 5px 10px;
  }

  a:hover {
    background: rgba(0, 0, 0, 0.03);
    border-radius: 4px;
  }
</style>
