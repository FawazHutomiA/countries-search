<!-- Please remove this file from your project -->
<template>
  <div class="flex flex-col mt-60 lg:mt-48 items-center">
    <h1 class="mb-4 lg:mb-8 font-bold text-2xl lg:text-5xl">Country</h1>
    <div class="relative flex w-full max-w-xs lg:max-w-xl">
      <input
        v-model="country"
        type="text"
        placeholder="Type any country name"
        class="relative input input-bordered pl-4 focus:input-primary w-full max-w-xs lg:max-w-xl"
      />
      <div class="flex absolute inset-y-0 right-0 items-center pr-4">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-5 w-5 text-gray-500"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
          />
        </svg>
      </div>
    </div>
    <div
      v-if="countries.length > 0"
      class="border shadow-md rounded mt-1 w-full max-w-xs lg:max-w-xl"
    >
      <ul class="pl-4 py-2 cursor-pointer">
        <li v-for="(item, key) in countries" :key="key">
          <nuxt-link :to="`/detail/${item.name}`">{{ item.name }}</nuxt-link>
        </li>

        <!-- <li
          v-for="(item, key) in countries"
          :key="key"
          @click="goToDetailCountry"
        >
          {{ item.name.common }}
        </li> -->
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      error: '',
      searchQuery: '',
      selectedItem: null,
      isVisible: false,
      country: '',
      countries: [],
    }
  },
  watch: {
    country: {
      handler(val) {
        console.log(val)
        if (val.length > 2) {
          setTimeout(() => {
            this.fetchCountries()
          }, 500)
        } else {
          this.countries = []
        }
      },
    },
  },
  methods: {
    async fetchCountries() {
      await fetch(`https://restcountries.com/v2/name/${this.country}`)
        .then((response) => response.json())
        .then((data) => (this.countries = data))
    },
    goToDetailCountry(val) {
      this.router.push(`/detail/${val}`)
    },
  },
}
</script>
