<template>
  <div class="max-w-screen-xl mx-auto">
    <MainHeader/>
    <router-view
    :inventory="inventory"
    :addInv="addInventory"
    :updateInv="updateInventory"
    :removeInv="removeInventory"
    />
    <MainFooter/>
  </div>
</template>

<script>
import MainFooter from './components/MainFooter.vue'
import MainHeader from './components/MainHeader.vue'
import ProductDataService from '@/services/ProductDataService'
export default {
  components: {
    MainHeader,
    MainFooter
  },
  mounted () {
    ProductDataService.getAll()
      .then(response => {
        this.inventory = response.data
      })
  },
  data () {
    return {
      showSidebar: false,
      inventory: [],
      cart: {}
    }
  },
  methods: {
    addInventory (product) {
      this.inventory.push(product)
    },
    updateInventory (index, data) {
      this.inventory[index].name = data.name
      this.inventory[index].photo = data.photo
      this.inventory[index].price = data.price
      this.inventory[index].description = data.description
      this.inventory[index].category = data.category
    },
    removeInventory (index) {
      this.inventory.splice(index, 1)
    }
  }
}
</script>
