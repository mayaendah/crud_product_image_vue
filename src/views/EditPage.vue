<template>
  <div class="container">
    <div class="card border-primary mt-5 shadow mt-4">
      <div class="card-body">
        <div class="row">
          <div class="col">
            <h4><span class="badge bg-primary">Ubah Data Product</span></h4>
          </div>
        </div>
        <div class="row mt-2">
          <div class="col p-4">
            <form @submit.prevent="updateproduct">
              <div class="form-group row">
                <label class="col-sm-2 col-form-label">Nama Pesanan</label>
                <input type="text" class="form-control" v-model="title">
              </div>
              <div class="form-group row">
                <label class="col-sm-2 col-form-label">Harga Pesanan</label>
                <input type="text" class="form-control" v-model="price">
              </div>
              <div class="form-group">
                <label for="formFile" class="form-label mt-2">Gambar Pesanan</label>
                <input class="form-control" type="file" @change="imgupload">
                <input class="form-control" type="text" v-model="image" >
              </div>
              <div class="form-group mt-4">
                
                <img :src="`http://localhost:8000/storage/${image}`" alt="" width="200"/>
              </div>
              <button type="submit" class="btn btn-primary" style="float:right">Submit</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref,onMounted} from 'vue';
import { useRoute} from 'vue-router';
import axios from 'axios';


const title=ref('');
const price=ref('');
const image=ref('');
const new_image=ref('');

const route=useRoute()

const getProductById=async()=>{
  let url = `http://127.0.0.1:8000/api/crud/${route.params.id}`;
      await axios.get(url).then(response => {
        title.value = response.data.data.title;
        price.value = response.data.data.price;
        image.value = response.data.data.image;
       

      }).catch(error => {
        console.log(error);
      });
}

const imgupload=async(e)=>{
  new_image.value=e.target.files[0];
}

const updateproduct=async()=>{
  const formData=new FormData();
  formData.append('title',title.value);
  formData.append('price',price.value);
  formData.append('image',image.value);
  formData.append('new_image',new_image.value);
  const res=await axios.post(`http://127.0.0.1:8000/api/crud/update/${route.params.id}`,formData);
  console.log(res);
        if (res){
          alert("update berhasil");
        }
}

onMounted(()=>{
  getProductById();
  
})
</script>


<!-- <script>
import axios from 'axios';
export default {
  name: 'editpage',
  data() {
    return {
      products: {},
      preview: "",
      newImage:""
    }
  },
  created() {
    this.getProductById();
  },
  methods: {
    async getProductById() {

      let url = `http://127.0.0.1:8000/api/crud/${this.$route.params.id}`;
      await axios.get(url).then(response => {
        console.log(response);
        this.products = response.data.data;

      }).catch(error => {
        console.log(error);
      });
    },
    imgupload(e) {
      this.newImage = e.target.files[0];

      let fileReader = new FileReader();
      fileReader.readAsDataURL(this.newImage);
      fileReader.onload = e => {
        this.preview = e.target.result;
        // console.log(this.preview);
      }
    },
    async updateproduct() {
     
      let formData = new FormData();
      formData.append("title", this.products.title);
      formData.append("price", this.products.price);
      formData.append("image", this.products.image);
      formData.append("newImage", this.newImage);
         

          for (var pair of formData.entries()) {
            console.log(pair[0]+ ', ' + pair[1]); 
          }

        axios.put(`http://127.0.0.1:8000/api/crud/${this.$route.params.id}`,formData)
        .then((response) => {
          console.log(response);

          //redirect ke listpage
          // this.$router.push({ name: 'listpage' });

        }).catch(error => {
         
          console.log(error);
        });
      
        
    },
  }
}
</script> -->

