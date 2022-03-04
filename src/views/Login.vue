<template>
  <form @submit.prevent="login" class="form mt-5">
    <h2 class="form-heading">Login</h2>
    <input
      class="form-input border-inset"
      type="email"
      v-model="email"
      placeholder="Email"
    />
    <input
      class="form-input border-inset"
      type="password"
      v-model="password"
      placeholder="Password"
    />
    <div class="row">
      <div class="col-sm-5">
    <button type="submit" class="form-btn">Sign in</button>
      </div>
      <div class="col-sm-7">
    <p>
      Not a member?
      <a href="/register">Create an account</a>
    </p>
      </div>
    </div>
  </form>
</template>
<script>
export default {
  name: "Login",
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    login() {
      fetch("https://mmpos-group-api.herokuapp.com/users", {
        method: "PATCH",
        body: JSON.stringify({
          email: this.email,
          password: this.password,
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
        },
      })
        .then((response) => response.json())
        .then((json) => {
          console.log(json)
          localStorage.setItem("jwt", json.jwt);
          alert("User logged in");
          this.$router.push({ name: "Home" });
        })
        .catch((err) => {
          alert(err);
        });
    },
  },
};
</script>
<style scoped>
.border {
  border-radius: 20px;
  background: #f5f5f5;
}
.border-inset {
  border-bottom: 1px solid #000 !important;
}

.form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 40px;
  gap: 20px;
  width: 100%;
  margin-inline: auto;
  max-width: 600px;
}

.form-heading {
  text-align: center;
  text-transform: uppercase;
}

.form-input {
  border: none;
  outline: none;
  padding: 20px;
}

.form-btn {
  cursor: pointer;
  transition: all 0.1s linear;
  padding: 10px;
  border: none;
  background: #f8ad9d;
  float: left;
}
.form-btn:hover {
  background: #f4978e;
}
.col-sm-7 p {
  font-size: 1.1rem;
  margin-top: 20px;
}

.col-sm-7 a {
  color: #f8ad9d !important;
  transition: 00.2s;
}
.col-sm-7 a:hover {
  color: #333 !important;
  transition: 00.2s;
}


.form-social-login {
  display: flex;
  justify-content: space-between;
}

.form-social-btn {
  width: 45%;
  color: #333;
}
</style>
