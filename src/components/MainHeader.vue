<template>
  <div class="container mx-auto p-4 flex justify-between items-center bg-blue-500 text-white">
    <h1 class="text-lg font-bold">E-COMMERCE</h1>
    <nav>
      <ul class="flex space-x-4">
        <li>
          <router-link
            to="/"
            class="hover:text-blue-500 p-5"
            :class="{
              'text-blue-500': isCurrent('Home'),
              'bg-blue-200': isCurrent('Home'),
              'hover:bg-white hover:text-blue-500': !isCurrent('Home')
            }"
            @click="setActive('Home')"
            :aria-current="isCurrent('Home') ? 'page' : null">
            Accueil
          </router-link>
        </li>
        <li>
          <router-link
            to="/products"
            class="hover:text-blue-500 p-5"
            :class="{
              'text-blue-500': isCurrent('Products'),
              'bg-blue-200': isCurrent('Products'),
              'hover:bg-white hover:text-blue-500': !isCurrent('Products')
            }"
            @click="setActive('Products')"
            :aria-current="isCurrent('Products') ? 'page' : null">
            Products
          </router-link>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  methods: {
    setActive (option) {
      this.activeOption = option
    },
    isCurrent (routeName) {
      return this.$route.name === routeName
    }
  }
}
</script>
