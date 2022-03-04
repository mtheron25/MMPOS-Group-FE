<template>
<h1>Your Cart</h1>
<main id="cart" style="max-width:960px">
  <div class="container" v-if="cart"> 
    <div class="row align-items-start">
      <div class="col-12 col-sm-8 items">
        <!--1-->
        <div class="cartItem row align-items-start" v-for="carts of cart" :key="carts._id">
          <div class="col-3 mb-2">
            <img class="w-100" :src="carts.img" alt="art image">
          </div>
          <div class="col-5 mb-2">
            <h6 class=""></h6>
    
            <p class="pl-1 mb-0">Item: {{carts.title}}</p>
          </div>
          <div class="col-2" >
            <p class="cartItemQuantity p-1 text-center">Quantity: {{carts.qty}}</p>
          </div>
          <div class="col-2">
            <p id="cartItem1Price">PRICE: R{{carts.price}}</p>
          </div>
        </div>
        <hr>

        <a href="#"><button id="btn-checkout" class="shopnow"><span>Checkout</span></button></a>
      </div>
    </div>
  </div>
  
</main>
</template>

<script>
export default {
  data(){
    return {
      cart: null,
    }
  },
  mounted(){
    fetch("https://mmpos-group-api.herokuapp.com/users/cart", {
        method: "GET",
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          this.cart = json

          })
        .catch((err) => {
          alert(err);
        });
  }
}
</script>

<style scoped>
#cart {
  max-width: 1440px;

  margin: auto;
}
.form div {
  margin-bottom: 0.4em;
}
.cartItem {
  --bs-gutter-x: 1.5rem;
}
.cartItemQuantity,
.proceed {
  background: #f4f4f4;
}
.items {
  padding-right: 30px;
}
#btn-checkout {
  min-width: 100%;
}
.container{
  width: 100vw !important;
}

/* stasysiia.com */
@import url("https://fonts.googleapis.com/css2?family=Exo&display=swap");
body {
  background-color: #fff;
  font-family: "Exo", sans-serif;
  font-size: 22px;
  margin: 0;
  padding: 0;
  color: #111111;
  justify-content: center;
  align-items: center;
}
a {
  color: #0e1111;
  text-decoration: none;
}
.btn-check:focus + .btn-primary,
.btn-primary:focus {
  color: #fff;
  background-color: #111;
  border-color: transparent;
  box-shadow: 0 0 0 0.25rem rgb(49 132 253 / 50%);
}
button:hover,
.btn:hover {
  box-shadow: 5px 5px 7px #c8c8c8, -5px -5px 7px white;
}
button:active {
  box-shadow: 2px 2px 2px #c8c8c8, -2px -2px 2px white;
}

/*PREVENT BROWSER SELECTION*/
a:focus,
button:focus,
input:focus,
textarea:focus {
  outline: none;
}
/*main*/
main:before {
  content: "";
  display: block;
  height: 88px;
}
h1 {
  font-size: 2.4em;
  font-weight: 600;
  letter-spacing: 0.15rem;
  text-align: center;
  margin: 30px 6px;
  color: #000;
}
h2 {
  color: rgb(37, 44, 54);
  font-weight: 700;
  font-size: 2.5em;
}
h3 {
  border-bottom: solid 2px #000;
}
h5 {
  padding: 0;
  font-weight: bold;
  color: #92afcc;
}
p {
  color: #333;
  font-family: "Roboto", sans-serif;
  margin: 0.6em 0;
}
h1,
h2,
h4 {
  text-align: center;
  padding-top: 16px;
}
/* yukito bloody */
.back {
  position: relative;
  top: -30px;
  font-size: 16px;
  margin: 10px 10px 3px 15px;
}
.inline {
  display: inline-block;
}

.shopnow,
.contact {
  background-color: #000;
  padding: 10px 20px;
  font-size: 30px;
  color: white;
  text-transform: uppercase;
  letter-spacing: 1px;
  transition: all 0.5s;
  cursor: pointer;
}
.shopnow:hover {
  text-decoration: none;
  color: white;
  background-color: #c41505;
}
/* for button animation*/
.shopnow span {
  cursor: pointer;
  display: inline-block;
  position: relative;
  transition: all 0.5s;
}
.shopnow span:after {
  content: url("https://badux.co/smc/codepen/caticon.png");
  position: absolute;
  font-size: 30px;
  opacity: 0;
  top: 2px;
  right: -6px;
  transition: all 0.5s;
}
.shopnow:hover span {
  padding-right: 25px;
}
.shopnow:hover span:after {
  opacity: 1;
  top: 2px;
  right: -6px;
}
.ma {
  margin: auto;
}
.price {
  color: slategrey;
  font-size: 2em;
}
#mycart {
  min-width: 90px;
}
#cartItems {
  font-size: 17px;
}
#product .container .row .pr4 {
  padding-right: 15px;
}
#product .container .row .pl4 {
  padding-left: 15px;
}

</style>