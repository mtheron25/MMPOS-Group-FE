<template>
  <div class="home">
    <img src="https://cdn.pixabay.com/photo/2015/10/20/17/50/berlin-998177_960_720.jpg" alt="" class="bg-img">
    <div class="h-text">
<h1 class="display-4">Welcome To LC Boutique</h1>
<router-link :to="{ name: 'Login' }" class="home-signin" v-if="isLoggedIn !== false">SIGN IN</router-link>
<p v-else></p>
    </div>
  </div>
</template>

<script>

export default {
  props: ['isLoggedIn'],
  name: 'Home',
  components: {
  }
}
</script>
<style scoped>
.home{
  height: 100vh !important;
  width: auto;
}
.h-text {
  position: relative;
  margin-right: auto;
  margin-left: auto;
  bottom: 60%;
  background-color: rgb(126, 86, 86); /* Fallback color */
  background-color: rgba(128, 95, 95, 0.4); /* Black w/opacity/see-through */
  color: white;
  border: 3px solid #000;
  width: 80%;
  padding: 30px;
}
.bg-img {
   filter: blur(4px);
  -webkit-filter: blur(4px);
  background-position: center;
  background-size: cover;
  height: 100%;
  width: 100%;
}
.home-signin {
  text-decoration: none;
  font-size: 1.2em;
  border: 1px solid #000;
  padding: 5px;
  transition: 00.3s;
}
.home-signin:hover {
  background-color: rgb(244, 151, 142); 
  background-color: rgb(192, 114, 107);
  transition: 00.3s;
}
</style>