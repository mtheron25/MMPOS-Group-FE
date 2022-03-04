<template>
  <section class="section_proj">
    <div class="container">
      <div class="row">
        <div class="col">
          <h2 class="head d-flex float-end text-dark my-5">PRODUCTS</h2>
        </div>
        <div class="col">
          <!-- go to cart -->
          <a href="/cart" id="cart"
            ><i class="fas fa-shopping-bag fa-2x my-3 float-end"></i
          ></a>
          <!-- Button trigger modal for add product -->
          <a
            id="addproduct"
            style="cursor: pointer"
            data-bs-toggle="modal"
            data-bs-target="#addModal"
          >
            <i class="fas fa-plus fa-2x my-3 float-end me-4"></i>
          </a>
        </div>
      </div>
      <!-- modals -->
      <!-- Add product modal -->
      <div
        class="modal fade"
        id="addModal"
        tabindex="-1"
        aria-labelledby="addModalLabel"
        aria-hidden="true"
      >
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="addModalLabel">Add Product</h5>

              <button
                type="button"
                class="btn-close btn-danger"
                data-bs-dismiss="modal"
                aria-label="Close"
              ></button>
            </div>
            <div class="modal-body">
              <form @submit.prevent="createProduct">
                <ul>
                  <li>NAME</li>
                  <li><input v-model="title" required type="text" /></li>
                  <li>PRICE</li>
                  <li><input v-model="price" required type="number" /></li>
                  <li>DESCRIPTION</li>
                  <li><input v-model="description" type="text" /></li>
                  <li>IMG URL</li>
                  <li><input v-model="img" required type="text" /></li>
                  <label for="genre">CATEGORY: </label>
                  <select id="genre" v-model="category" name="genre">
                    <option value="Shoes">Shoes</option>
                    <option value="Accessories">Dresses</option>
                    <option value="Clothing">Swimwear</option>
                  </select>
                </ul>

                <div class="modal-footer">
                  <button
                    type="button"
                    class="btn btn-secondary"
                    data-bs-dismiss="modal"
                  >
                    Close
                  </button>
                  <button type="submit" class="btn btn-success">
                    Save changes
                  </button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
      <!-- Modal for edit  product -->
      <div
        class="modal fade"
        id="editModal"
        tabindex="-1"
        aria-labelledby="editModalLabel"
        aria-hidden="true"
      >
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="editModalLabel">Edit Product</h5>

              <button
                type="button"
                class="btn-close btn-danger"
                data-bs-dismiss="modal"
                aria-label="Close"
              ></button>
            </div>
            <div class="modal-body">
              <form @submit.prevent="editProduct">
                <ul>
                  <li>NAME</li>
                  <li><input v-model="title" type="text" /></li>
                  <li>PRICE</li>
                  <li><input v-model="price" type="number" /></li>
                  <li>DESCRIPTION</li>
                  <li><input v-model="description" type="text" /></li>
                  <li>IMAGE URL</li>
                  <li><input v-model="img" type="text" /></li>
                  <li>
                    <label for="genre">CATEGORY: </label>
                    <select id="genre" v-model="category" name="genre">
                      <option value="Shoes">Shoes</option>
                      <option value="Accessories">Dresses</option>
                      <option value="Clothing">Swimwear</option>
                    </select>
                  </li>
                </ul>
                <div class="modal-footer">
                  <button
                    type="button"
                    class="btn btn-secondary"
                    data-bs-dismiss="modal"
                  >
                    Close
                  </button>
                  <button type="submit" class="btn btn-success">
                    Save changes
                  </button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
      <!-- get all products -->

      <div class="row">
         <input type="text" v-model="search" class="search-input" placeholder="Search..">
        <div
          class="card col-md-4 col-sm-6 col-xs-12"
          v-for="product of filterProducts" :key="product._id"
        >
       
          <div class="imgBx">
            <img class="prod-img" :src="product.img" />
          </div>
          <div class="contentBx">
            <h4>{{ product.title }}</h4>
            <div class="info">
              <div class="desc text-dark">{{ product.description }}</div>
              <div class="category text-dark">{{ product.category }}</div>
              <div class="price text-dark">R {{ product.price }}</div>

              <div class="cart d-flex">
                <input
                  type="number"
                  class="quantity"
                  value="1"
                  id="addToCart"
                />
                <button
                  type="button"
                  class="btn btn-mute ms-3"
                  @click="addToCart(product._id)"
                >
                  <i class="fas fa-shopping-bag fa-2x"></i>
                </button>

                <button
                  type="button"
                  class="btn btn-mute ms-3"
                  data-bs-toggle="modal"
                  data-bs-target="#editModal"
                >
                  <i class="fas fa-wrench fa-2x"></i>
                </button>
                <button
                  type="button"
                  class="btn btn-mute ms-3"
                  @click="deleteProduct(id)"
                >
                  <i class="fas fa-trash fa-2x"></i>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <br />
    </div>
    <br />
  </section>
</template>
<script>
export default {
  data() {
    return {
      products: null,
      title: "",
      description: "",
      category: "",
      price: "",
      img: "",
      qty: 1,
      search: ""
    };
  },

  // fetching product
  created() {
    if (localStorage.getItem("jwt")) {
      fetch("https://mmpos-group-api.herokuapp.com/products", {
        method: "GET",
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          this.products = json;
          this.products.forEach(async (product) => {
            await fetch(
              "https://mmpos-group-api.herokuapp.com/products/" + product.title,
              {
                method: "GET",
                headers: {
                  "Content-type": "application/json; charset=UTF-8",
                  Authorization: `Bearer ${localStorage.getItem("jwt")}`,
                },
              }
            )
              .then((response) => response.json())
              .then((json) => {
                product.author = json.title;
              });
          });
        })
        .catch((err) => {
          alert("Log in failed");
        });
    } else {
      alert("Not logged in");
      this.$router.push({ name: "Login" });
    }
  },
  methods: {
    createProduct() {
      if (!localStorage.getItem("jwt")) {
        alert("User not logged in");
        return this.$router.push({ name: "Login" });
      }
      fetch("https://mmpos-group-api.herokuapp.com/products", {
        method: "POST",
        body: JSON.stringify({
          title: this.title,
          description: this.description,
          category: this.category,
          price: this.price,
          img: this.img,
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          alert("Product Created");
          this.$router.push({ name: "Products" });
        })
        .catch((err) => {
          alert(err);
        });
    },
    addToCart(id) {
      fetch(`https://mmpos-group-api.herokuapp.com/users/${id}/cart`, {
        method: "POST",
        body: JSON.stringify({
          qty: this.qty,
        }),
        headers: {
          "Content-type": "application/json; charset=UTF-8",
          Authorization: `Bearer ${localStorage.getItem("jwt")}`,
        },
      })
        .then((response) => response.json())
        .then((json) => {
          alert("Added to Cart");
          this.$router.push({ name: "Products" });
        })
        .catch((err) => {
          alert(err);
        });
    }
  },
  computed:{
    filterProducts:function(){
      return this.products.filter((product)=>{
        return product.title.match(this.search)
      })
    }
    
  }
};
</script>
<style scoped>
* {
  box-sizing: border-box;
}

ul {
  list-style: none;
}
.modal-body {
  z-index: 200;
}
.quantity {
  width: 40px;
  height: 35px;
  margin-top: 6px !important;
  border-radius: 5px;
}
.head {
  letter-spacing: 6px;
  text-align: center;
  transform: scale(0.95, 1);
  color: #111 !important;
}
.section_proj {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: white;
}
.price {
  font-weight: 500 !important;
  margin-bottom: 12px !important;
}

.fas:hover {
  color: #f4978e !important;
}
.container .card {
  /* position: relative; */
  margin-right: 40px !important;
  margin-left: 40px !important;
  margin-top: 20px !important;
  width: 280px;
  height: 450px;
  background: rgb(221, 200, 200);
  border-radius: 20px;
  overflow: hidden;
}
.button1 {
  background-color: transparent !important;
  margin-left: 10px;
  float: left;
  font-size: 30px !important;
}
.button2 {
  background-color: transparent !important;
  margin-right: 10px;
  float: right;
  font-size: 30px !important;
}

.container .card:before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #f8ad9d;
  clip-path: circle(150px at 80% 20%);
  transition: 0.5s ease-in-out !important;
}

.container .card:hover:before {
  clip-path: circle(300px at 80% -20%);
}

.container .card .imgBx {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  z-index: 100;
  width: 100%;
  height: 220px;
  transition: 0.5s;
  margin-right: 50px !important;
}

.container .card:hover .imgBx {
  top: 0%;
  transform: translateY(0%);
}

.container .card .imgBx img {
  position: absolute;
  top: 50%;
  left: 47%;
  transform: translate(-50%, -50%);
  width: 190px;
  height: 200px;
  object-fit: inherit;
}

.prod-img {
  -webkit-border-radius: 30px;
  -moz-border-radius: 30px;
  border-radius: 30px;
  transition-delay: 0.4s;
}

.container .card .contentBx {
  position: absolute;
  bottom: 0;
  width: 100%;
  height: 120px;
  text-align: center;
  transition: 1s;
  z-index: 10;
  padding-right: 25px !important;
}

.container .card:hover .contentBx {
  height: 210px;
}

.container .card .contentBx h4 {
  position: relative;
  font-weight: 500;
  letter-spacing: 1px;
  color: #fff;
  margin: 0;
}

.container .card .contentBx .size,
.container .card .contentBx .color {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 8px 20px;
  transition: 0.3s;
  opacity: 0;
  visibility: hidden;
  padding-top: 0;
  padding-bottom: 0;
}

.container .card:hover .contentBx .size {
  opacity: 1;
  visibility: visible;
  transition-delay: 0.3s;
}

.container .card:hover .contentBx .color {
  opacity: 1;
  visibility: visible;
  transition-delay: 0.3s;
}

.container .card .contentBx .size h3,
.container .card .contentBx .color h3 {
  font-weight: 300;
  font-size: 14px;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-right: 10px;
}

.container .card .contentBx .size span {
  width: 26px;
  height: 26px;
  text-align: center;
  line-height: 26px;
  font-size: 14px;
  display: inline-block;
  color: #111;

  margin: 0 5px;
  transition: 0.3s;
  color: #111;
  border-radius: 4px;
  cursor: pointer;
}

.container .card .contentBx a {
  display: inline-block;
  padding: 10px 20px;
  background: #fff;
  border-radius: 4px;
  margin-top: 10px;
  text-decoration: none;
  font-weight: 600;
  color: #111;
  opacity: 0;
  transform: translateY(50px);
  transition: 0.3s;
  margin-top: 0;
}

.container .card:hover .contentBx a {
  opacity: 1;
  transform: translateY(0px);
  transition-delay: 0.3s;
}

#cart {
  float: right;
  margin-top: 30px;
  margin-right: -50px;
}
#addproduct {
  float: right;
  margin-top: 30px;
  margin-right: -20px;
}
@media (max-width: 500px) {
  .section_proj {
    width: 112%;
  }
}
</style>
