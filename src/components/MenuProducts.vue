<template>
<form>
  <div class="container mx-auto space-y-12 mt-10">
    <div class="border-b border-gray-900/10 pb-6">
      <h1 class="text-4xl font-bold mb-4">Products</h1>

      <div class="mt-10 grid grid-cols-1 gap-x-2 gap-y-2 sm:grid-cols-12">

        <div class="sm:col-span-6">
          <label for="name" class="block text-sm font-medium leading-6 text-gray-900">Name</label>
          <div class="mt-2">
            <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600 sm:max-w-md">
              <input type="text" name="name" id="name" v-model="product.name" class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6">
            </div>
          </div>
        </div>

        <div class="sm:col-span-12">
          <label for="description" class="block text-sm font-medium leading-6 text-gray-900">Description</label>
          <div class="mt-2">
            <textarea id="description" name="description" rows="3" v-model="product.description" class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"></textarea>
          </div>
        </div>

        <div class="sm:col-span-4">
          <label for="price" class="block text-sm font-medium leading-6 text-gray-900">Price</label>
          <div class="mt-2">
            <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600 sm:max-w-md">
              <input type="number" step="0.01" name="price" id="price" v-model="product.price" class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6">
            </div>
          </div>
        </div>

        <div class="sm:col-span-4">
          <label for="photo" class="block text-sm font-medium leading-6 text-gray-900">Photo</label>
          <div class="mt-2">
            <div class="flex rounded-md shadow-sm ring-1 ring-inset ring-gray-300 focus-within:ring-2 focus-within:ring-inset focus-within:ring-indigo-600 sm:max-w-md">
              <input type="text" name="photo" id="photo" v-model="product.photo" class="block flex-1 border-0 bg-transparent py-1.5 pl-1 text-gray-900 placeholder:text-gray-400 focus:ring-0 sm:text-sm sm:leading-6">
            </div>
          </div>
        </div>

        <div class="sm:col-span-4">
          <label for="category" class="block text-sm font-medium leading-6 text-gray-900">Category</label>
          <div class="mt-2">
            <select id="category" name="category" v-model="product.category" class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:max-w-xs sm:text-sm sm:leading-6">
              <option value="">Select</option>
              <option value="1">Category 1</option>
              <option value="2">Category 2</option>
              <option value="3">Category 3</option>
            </select>
          </div>
        </div>

        <div class="mt-2">
          <button v-if="isEditing" @click="saveEditedProduct" class="rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Save</button>
          <button v-else @click="saveProduct" class="rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Add</button>
        </div>

      </div>
    </div>
  </div>
</form>

<table class="min-w-full mt-4">
  <thead>
    <tr class="bg-gray-300 h-12">
      <th>Name</th>
      <th>Description</th>
      <th>Price</th>
      <th>Photo</th>
      <th>Category</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <template v-if="inventory.length > 0">
      <tr v-for="(product, i) in inventory" :key="i" class="h-12">
        <td>{{ product.name }}</td>
        <td>{{ product.description }}</td>
        <td>{{ product.price }}</td>
        <td>{{ product.photo }}</td>
        <td>{{ product.category }}</td>
        <td class="flex pt-1.5 gap-2">
          <button type="button" @click="editProduct(product)" class="rounded-md bg-amber-500 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-amber-400 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Edit</button>
          <button type="button" @click="deleteProduct(product.id)" class="rounded-md bg-red-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-red-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Delete</button>
        </td>
      </tr>
    </template>
    <template v-else>
      <tr class="h-12">
        <td colspan="6">No products available.</td>
      </tr>
    </template>
  </tbody>
</table>
</template>

<script>
import ProductDataService from '@/services/ProductDataService'

export default {
  props: {
    inventory: {
      type: Array,
      required: true
    },
    addInv: {
      type: Function,
      required: true
    },
    updateInv: {
      type: Function,
      required: true
    },
    removeInv: {
      type: Function,
      required: true
    }
  },
  data () {
    return {
      product: {
        name: '',
        description: '',
        price: 0,
        photo: '',
        category: ''
      },
      submitted: false,
      message: '',
      isEditing: false
    }
  },
  methods: {
    saveProduct () {
      if (this.isEditing) {
        ProductDataService.update(this.product.id, this.product)
          .then(() => {
            this.updateInv(this.findIndexById(this.product.id), this.product)
            this.isEditing = false
            this.clearForm()
          })
          .catch((e) => {
            console.log(e)
            this.message = e.response.data.message
          })
      } else {
        ProductDataService.create(this.product)
          .then((response) => {
            this.product.id = response.data.id
            this.addInv(this.product)
            this.submitted = true
            this.message = null
            this.clearForm()
          })
          .catch((e) => {
            console.log(e)
            this.message = e.response.data.message
          })
      }
    },
    editProduct (selectedProduct) {
      this.product = { ...selectedProduct }
      this.isEditing = true
    },
    saveEditedProduct () {
      this.saveProduct()
    },
    deleteProduct (productId) {
      ProductDataService.delete(productId)
        .then(() => {
          this.removeInv(this.findIndexById(productId))
        })
        .catch((e) => {
          console.log(e)
          this.message = e.response.data.message
        })
    },
    clearForm () {
      this.product = {
        name: '',
        description: '',
        price: 0,
        photo: '',
        category: ''
      }
      this.submitted = false
      this.message = ''
      this.isEditing = false
    },
    findIndexById (id) {
      return this.inventory.findIndex((product) => product.id === id)
    }
  }
}
</script>
