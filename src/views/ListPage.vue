<template>
  <div class="container">
    <div class="card border-primary mt-5 shadow">
      <div class="card-body">
        <div class="row">
          <div class="col-md-4" v-for="product in products" :key="product.id">
            <div class="card mt-4 p-4">
              <center>
                <img
                  :src="`http://localhost:8000/storage/${product.image}`"
                  width="200"
                  height="200"
                />
              </center>
              <div class="card-body" align="right">
                <h5 class="card-title">{{ product.title }}</h5>
                <p class="card-text">Harga Rp. {{ product.price }}</p>

                <router-link
                  class="btn btn-primary btn-sm rounded shadow"
                  :to="{ name: 'editpage', params: { id: product.id } }"
                  >Edit</router-link
                >&nbsp;

                <button
                  class="btn btn-danger btn-sm rounded shadow"
                  @click.prevent="delProduct(product.id)"
                >
                  Delete
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      products: Array,
    };
  },
  created() {
    this.getProduct();
  },
  methods: {
    async getProduct() {
      let url = "http://127.0.0.1:8000/api/crud";
      await axios
        .get(url)
        .then((response) => {
          console.log(response);
          this.products = response.data.data;
          console.log(this.products);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    async delProduct(id) {
      let url = `http://127.0.0.1:8000/api/crud/${id}`;
      await axios
        .delete(url)
        .then((response) => {
          if (response.data.code == 200) {
            alert(response.data.message);
            // this.getProduct();

            // splice posts
            const index = this.products.findIndex((product) => product.id === id); // find the post index
            if (~index) {
              // if the post exists in array
              this.products.splice(index, 1);
            }
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  mounted() {
    console.log("Product List created");
  },
};
</script>`