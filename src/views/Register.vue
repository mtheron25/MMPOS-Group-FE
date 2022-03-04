<template>
<section class="register">
  <form @submit.prevent="register" class="form">
    <h2 class="form-heading">Register</h2>
    <input
      class="form-input border-inset"
      type="text"
      v-model="name"
      placeholder="Name"
      required
    />
    <input
      class="form-input border-inset"
      type="email"
      v-model="email"
      placeholder="Email"
      required
    />
    <input
      class="form-input border-inset"
      type="text"
      v-model="contact"
      placeholder="Contact Number"
      required
    />
    <input
      class="form-input border-inset"
      type="password"
      v-model="password"
      placeholder="Password"
      required
    />
    <div class="row">
      <div class="col-sm-5">
    <button type="submit" class="form-btn">Sign up</button>
      </div>
      <div class="col-sm-7">
    <p>
      Already a member?
      <a href="/login">Sign in</a>
    </p>
      </div>
    </div>
  </form>
  </section>
</template>
<script>
export default {
  data() {
    return {
      name: "",
      email: "",
      contact: "",
      password: "",
    };
  },
  methods: {
    register() {
      fetch("https://mmpos-group-api.herokuapp.com/users", {
        method: "POST",
        body: JSON.stringify({
          fullname: this.name,
          email: this.email,
          contact: this.contact,
          password: this.password,
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
        },
      })
        .then((response) => response.json())
        .then((json) => {
          alert("User registered");
          localStorage.setItem("jwt", json.jwt);
          this.$router.push({ name: "Login" });
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
  max-width: 600px;
  margin-top: 25px;
  margin-inline: auto;
}

.form-heading {
  text-align: center;
  text-transform: uppercase;
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

.form-input {
  border: none;
  outline: none;
  padding: 20px;
}

.form-btn {
  background: #f8ad9d;
  cursor: pointer;
  transition: all 0.1s linear;
  padding: 10px;
  border: none;
  float: left;
}

.form-btn:hover {
  background: #f4978e;
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
