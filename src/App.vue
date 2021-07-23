<template>
  <div class="block" v-if="isLoaded">

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat&display=swap" rel="stylesheet">

    <div class="left">
      <img :src="'https://www.piecoweb.com/media/catalog/product' + data._source.media_gallery[0].image" alt="Product">
    </div>
    <div class="right">
      <h2 class="price">{{data._source.final_price_incl_tax}} €</h2>
      <h1>{{ data._source.name }}</h1>
      <div v-html="data._source.description"></div>
      <div class="order">
        <input type="number" v-model="quantity">
        <button v-on:click="cart = !cart">>>> Add to cart</button>
      </div>
      <div class="category">
        <p>Product category :</p>
        <span v-for="category in data._source.category" :key="category.category_id"> {{category.name}}</span>
      </div>
      <p>Quantity available : {{ data._source.stock.qty}}</p>
    </div>

    <div class="cart" v-if="cart == true">
      <button v-on:click="cart = !cart">X</button>
      <p>This item has been added to your order !</p>
    </div>

  </div>
</template>

<script>
  import axios from 'axios'


  export default {
    name: 'App',
    data() {
      return {
        isLoaded: false,
        data: null,
        quantity: 0,
        cart: false
      }
    },
    mounted() {
      axios.get('https://demo.storefrontcloud.io/api/catalog/vue_storefront_catalog/product/_search?q=sku:24-MB02')
        .then((response) => {
          this.data = response.data.hits.hits[0]
          this.isLoaded = !this.isLoaded
        })
        .catch((error) => {
          if (error.response) {
            console.log(error.response.status)
          } else {
            console.log('Error', error.message)
          }
          console.log(error.config)
        })
    }
  }
</script>

<style scoped>
  .block {
    display: flex;
    flex-direction: row;
    margin: 0 auto;
    width: 80%;
    background-color: #E6DAD7 ;
    font-family: 'Montserrat', sans-serif;
  }

  .left {
    width: 50%;
    display: flex;
    flex: 0 0 auto;
  }

  .left img{
    max-width: 100%;
    max-height: 100%;
    margin: 0 auto;
  }

  .right {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    text-align: center;
    width: 50%;
    padding: 3%;
  }

  .order{
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    margin: 4% 0;
  }

  .order input{
    width: 10%;
  }

  .order input, .order button{
    cursor: pointer;
    border-radius: 8px;
    padding: 5px 10px;
  }

  .price{
    margin-left: 50%;
  }

  .category{
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
  }

  .category span {
    padding: 0 5px;
  }

  .cart{
    position: absolute;
    width: 250px;
    height: 150px;
    background-color: #CCC2BF;
    border: 1px solid black;
    border-radius: 10px;
    top: 50%;
    left: 50%;
    margin-left: -125px;
    margin-top: -75px;
    text-align: center;
  }

  .cart button{
    float: right;
    margin: 5px;
  }

  .cart p {
    margin-top: 25%;
  }

  @media screen and (max-width: 900px) {
  .block {
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
}
</style>