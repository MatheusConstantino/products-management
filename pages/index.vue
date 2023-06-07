<template>
  <div>
    <SideBar />
    <div class="main-content">
      <div class="background-main-container">
        <img src="../assets/image/login-bg.png" alt="" style="width: 100%">
        <div class="title-links-page">
          <h3>Home / Gestão de produtos</h3>
          <p class="mb-0">Home</p>
        </div>
        <div class="profile-container">
          <img src="../assets/image/jhon.png" class="image-profile" />
          <div class="profile-info pl-3">
            <strong>Jhon Doe</strong> <br>
            <small>john.doe@example.com</small>
          </div>
        </div>
      </div>

      <div class="table-data-container margin-top">
        <div class="card border-rounded">
          <div class="card-header bg-transparent">
            <div class="card-title float-left text-bold pt-2">
              <span class="card-title"> Gestão de produtos</span>
            </div>
            <div class="card-btns">
              <a class="btn btn-outline-purple float-right" @click.prevent="editOrCreateProduct()">Adicionar produto</a>
            </div>
          </div>
          <div class="card-body">
            <table class="w-100">
              <thead>
                <tr>
                  <th scope="" class="pb-2">Image</th>
                  <th scope="" class="pb-2">ID</th>
                  <th scope="" class="pb-2">Name</th>
                  <th scope="" class="pb-2">Price</th>
                  <th scope="" class="pb-2">ISBN</th>
                  <th scope="" class="pb-2"></th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(product, index) in products" :key="index">
                  <td>
                    <img v-if="product.images[0]" :src="baseUrl + product.images[0].image_path" alt=""
                      class="image-box" />
                    <img v-else src="https://fakeimg.pl/50x50/" class="image-box">
                  </td>
                  <td>{{ product.id }}</td>
                  <td>{{ product.name }}</td>
                  <td>{{ product.price }}</td>
                  <td>{{ product.isbn }}</td>
                  <td>
                    <a class="btn" @click="editOrCreateProduct(product)"><span
                        class="material-icons text-purple">edit</span>
                    </a>
                    <a class="btn" @click="deleteProduct(product.id)"><span
                        class="material-icons text-purple">delete</span></a>
                  </td>
                </tr>
              </tbody>
            </table>
            <div class="card-footer bg-transparent">
              <nav aria-label="Page navigation" class="float-right mr-3">
                <ul class="pagination">
                  <li class="page-item"><a class="page-link" href="#" @click.prevent="downPage()"><span
                        class="material-icons text-purple">chevron_left</span></a></li>
                  <li class="page-item"><a class="page-link" href="#" @click.prevent="upPage()"><span
                        class="material-icons text-purple">chevron_right</span></a></li>
                </ul>
              </nav>
              <div class="pagination-info float-right pt-3 mr-4">
                Página {{ currentPage }} de {{ totalPages }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <b-modal ref="edit-modal" title="Editar produto" class="border-rounded">
      <div class="modal-body">
        <div class="form">
          <div class="row">
            <div class="col-4">
              <label for="">Nome</label>
              <input type="text" class="input-rounded" v-model="productArray.name">
            </div>
            <div class="col-4">
              <label for="">Preço</label>
              <input type="number" class="input-rounded" v-model="productArray.price">
            </div>
            <div class="col-4">
              <label for="">ISBN</label>
              <input type="text" class="input-rounded" v-model="productArray.isbn">
            </div>
            <div class="col-12 mt-3">
              <label for="">Upload de imagens</label>
              <div class="file-input-container">
                <span v-if="blockMessage.length > 0">{{ blockMessage }}</span>
                <input type="file" class="input-rounded" id="customFile" @change="uploadImage($event)"
                  :disabled="disableInput" />
                <span v-if="uploadStatusMessage.length > 0">{{ uploadStatusMessage }}</span>
              </div>
              <div class="w-100 image-container mt-4">
                <div v-for="(image, index) in productArray.images" class="image-card">
                  <img :src="baseUrl + image.image_path" class="image-box">
                  <small>{{ productArray.name }}</small>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <template #modal-footer>
        <div class="w-100">
          <button class="btn btn-confirm float-right" @click="editProduct(productArray.id)">Editar</button>
          <button class="btn btn-cancel float-right mr-3" @click="$refs['edit-modal'].hide()">Cancelar</button>
        </div>
      </template>
    </b-modal>

    <b-modal ref="create-modal" title="Criar produto" class="border-rounded">
      <div class="modal-body">
        <div class="form">
          <div class="row">
            <div class="col-4">
              <label for="">Nome</label>
              <input type="text" class="input-rounded" v-model="productArray.name">
            </div>
            <div class="col-4">
              <label for="">Preço</label>
              <input type="number" class="input-rounded" v-model="productArray.price">
            </div>
            <div class="col-4">
              <label for="">ISBN</label>
              <input type="text" class="input-rounded" v-model="productArray.isbn">
            </div>
            <div class="col-12 mt-3">
              <label for="">Upload de imagens</label>
              <div class="file-input-container">
                <span v-if="blockMessage.length > 0">{{ blockMessage }}</span>
                <input type="file" class="input-rounded" id="customFile" @change="uploadImage($event)"
                  :disabled="disableInput" />
                <span v-if="uploadStatusMessage.length > 0">{{ uploadStatusMessage }}</span>
              </div>
              <div class="w-100 image-container mt-4">
                <div v-for="(image, index) in productArray.images" class="image-card">
                  <img :src="baseUrl + image.image_path" class="image-box">
                  <small>{{ productArray.name }}</small>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <template #modal-footer>
        <div class="w-100">
          <button class="btn btn-confirm float-right" @click="createProduct()">Criar</button>
          <button class="btn btn-cancel float-right mr-3" @click="$refs['create-modal'].hide()">Cancelar</button>
        </div>
      </template>
    </b-modal>
    <Toast v-if="show" :Title="Title" :Desc="Desc" :Type="Type" />
  </div>
</template>

<script>
import axios from 'axios'
import Toast from '@/components/Toast.vue'
import SideBar from '@/components/SideBar.vue'
import Cookies from 'js-cookie'
export default {
  components: { Toast, SideBar },
  data() {
    return {
      permitPage: true,
      token: '',
      products: [],
      lastPage: 0,
      firstPage: 1,
      currentPage: 1,
      totalPages: 0,
      baseUrl: 'http://127.0.0.1:8000',
      show: false,
      Title: '',
      Desc: '',
      Type: '',
      loading: false,
      productArray: {
        id: '',
        name: '',
        isbn: '',
        price: '',
        description: '',
        images: [],
      },
      typesArray: ['png', 'jpg', 'jpeg'],
      uploadStatusMessage: '',
      imagesIds: [],
      disableInput: false,
      blockMessage: ''
    }
  },
  mounted() {

    this.listProducts()
    this.token = Cookies.get('token');
    if (!this.token) {
      alert('Faça login para continuar')
      window.location.href = 'http://localhost:3000/login'
    }
  },
  methods: {
    async listProducts(page) {
      this.loading = true
      this.imagesIds = []
      const currentPage = !page ? 1 : page
      await axios.get(`http://127.0.0.1:8000/api/products?page=${currentPage}`, {
        headers: {
          Authorization: `Bearer ${this.token}`
        }
      }).then((response) => {
        this.products = response.data.data
        this.lastPage = response.data.pagination.last_page
        this.currentPage = response.data.pagination.current_page
        this.totalPages = response.data.pagination.last_page
        this.loading = false
      }).catch((error) => {
        console.error(error)
        this.loading = false
        this.Title = 'Oops!'
        this.Desc = 'An error has ocurred!'
        this.Type = 'D'
        this.show = true
        setTimeout(() => {
          this.show = false
        }, 2000);
      })
    },

    async upPage() {
      this.loading = true
      this.currentPage = this.currentPage + 1
      await this.listProducts(this.currentPage)
    },
    async downPage() {
      this.loading = true
      this.currentPage = this.currentPage - 1
      await this.listProducts(this.currentPage)
    },
    async deleteProduct(productId) {
      await axios.delete(`http://127.0.0.1:8000/api/products/${productId}`, {
        headers: {
          Authorization: `Bearer ${this.token}`
        }
      }).then((response) => {
        this.Title = 'Yay!'
        this.Desc = 'Product deleted Sucessfully!'
        this.Type = 'S'
        this.show = true
        setTimeout(() => {
          this.show = false
        }, 2000);
        this.listProducts(this.currentPage)
      }).catch((error) => {
        console.error(error)
        this.loading = false
        this.Title = 'Oops!'
        this.Desc = 'An error has ocurred!'
        this.Type = 'D'
        this.show = true
        setTimeout(() => {
          this.show = false
        }, 2000);
      })
    },
    async editOrCreateProduct(product) {
      if (product) {
        this.productArray.id = product.id,
          this.productArray.name = product.name,
          this.productArray.isbn = product.isbn,
          this.productArray.price = product.price,
          this.productArray.description = product.description,
          this.productArray.images = product.images
        this.productArray.images.map(element => {
          this.imagesIds.push(element.image_id)
        })
        if (this.productArray.images.length >= 5) {
          this.disableInput = true
          this.blockMessage = "Você pode apenas enviar 5 arquivos por produto"
        }
        return this.$refs['edit-modal'].show()
      } else {
        this.productArray.id = "",
          this.productArray.name = "",
          this.productArray.isbn = "",
          this.productArray.price = "",
          this.productArray.description = "",
          this.productArray.images = []
        this.disableInput = false
        this.blockMessage = ""
        return this.$refs['create-modal'].show()
      }
    },

    async createProduct() {
      const data = {
        name: this.productArray.name,
        isbn: this.productArray.isbn,
        price: this.productArray.price,
        description: this.productArray.description,
        image_ids: this.imagesIds
      }
      await axios.post(`http://127.0.0.1:8000/api/products`, data, {
        headers: {
          Authorization: `Bearer ${this.token}`
        }
      }).then((response) => {
        this.listProducts(this.lastPage)
        this.$refs['create-modal'].hide()
        this.Title = 'Yay!'
        this.Desc = 'Product created!'
        this.Type = 'S'
        this.show = true
        setTimeout(() => {
          this.show = false
        }, 2000);
      }).catch((error) => {
        console.error(error)
        this.Title = 'Oops!'
        this.Desc = 'An error has ocurred!'
        this.Type = 'D'
        this.show = true
        setTimeout(() => {
          this.show = false
        }, 2000);
      })
    },
    async editProduct(productId) {
      const data = {
        name: this.productArray.name,
        isbn: this.productArray.isbn,
        price: this.productArray.price,
        description: this.productArray.description,
        image_ids: this.imagesIds
      }
      await axios.put(`http://127.0.0.1:8000/api/products/${productId}`, data, {
        headers: {
          Authorization: `Bearer ${this.token}`
        }
      }).then((response) => {
        this.listProducts(this.currentPage)
        this.$refs['edit-modal'].hide()
        this.Title = 'Yay!'
        this.Desc = 'Product edited!'
        this.Type = 'S'
        this.show = true
        setTimeout(() => {
          this.show = false
        }, 2000);
      }).catch((error) => {
        console.error(error)
        this.Title = 'Oops!'
        this.Desc = 'An error has ocurred!'
        this.Type = 'D'
        this.show = true
        setTimeout(() => {
          this.show = false
        }, 2000);
      })
    },
    async logout() {
      Cookies.remove('token')
      window.location.href = 'http://localhost:3000/login'
    },
    async uploadImage(event) {
      const file = event.target.files[0]
      const fileName = file.name.split('.')
      const fileSize = Math.round(((file.size / 1024) / 1024))
      const fileExtension = fileName[fileName.length - 1]
      if (fileSize > 5 || this.typesArray.includes(fileExtension) === false) {
        this.uploadStatusMessage = "Arquivo muito grande ou formato não suportado.";
        return;
      }
      const formData = new FormData();
      formData.append('image', file);
      await axios.post('http://127.0.0.1:8000/api/upload-image', formData).then((response) => {
        this.imagesIds.push(response.data.image_id)
        this.productArray.images.push({
          image_id: response.data.image_id,
          image_path: response.data.image_path
        })
        if (this.imagesIds.length >= 5) {
          this.blockMessage = "Você pode apenas enviar 5 arquivos por produto"
          return this.disableInput = true
        }
      }).catch((error) => {
        console.error(error)
        this.Title = 'Oops!'
        this.Desc = 'An error has ocurred!'
        this.Type = 'D'
        this.show = true
        setTimeout(() => {
          this.show = false
        }, 2000);
      })
    },
  }

}
</script>
