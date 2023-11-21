<template>
  <div class="bg-slate-50 p-5 flex flex-col gap-y-5">
    <div class="text-2xl uppercase">Techbodia</div>

    <div class="bg-white border border-slate-200 rounded-md p-4">
      <form>
        <div
          class="flex flex-col gap-y-4 items-end md:flex-row md:items-center justify-around gap-x-3"
        >
          <input
            type="text"
            class="w-full md:w-auto flex-1 bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
            placeholder="Please enter country name"
            v-model="searchTerm"
            @input="handleSearchInput"
          />
          <button
            type="submit"
            class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
          >
            Submit
          </button>
        </div>
      </form>
    </div>

    <div class="flex flex-col gap-y-5 bg-white border border-slate-200 rounded-md p-4">
      <div class="overflow-x-auto">
        <table
          class="table-auto w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400"
        >
          <thead class="text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
            <tr class="border-b">
              <th scope="col" class="sticky left-0 z-10 px-6 py-4 bg-gray-50 dark:bg-gray-700">
                Flag
              </th>
              <th scope="col" class="px-6 py-4">
                <div class="flex items-center gap-x-1">
                  <span>Country</span>
                  <a href="" @click.prevent="handleSort">
                    <svg
                      class="w-3 text-slate-100 transition duration-300"
                      :class="{ 'rotate-180': sortDirection === 'asc' }"
                      viewBox="0 -4.5 24 24"
                      version="1.1"
                      xmlns="http://www.w3.org/2000/svg"
                      xmlns:xlink="http://www.w3.org/1999/xlink"
                      xmlns:sketch="http://www.bohemiancoding.com/sketch/ns"
                    >
                      <g
                        id="Page-1"
                        stroke="none"
                        stroke-width="1"
                        fill="none"
                        fill-rule="evenodd"
                        sketch:type="MSPage"
                      >
                        <g
                          id="Icon-Set-Filled"
                          sketch:type="MSLayerGroup"
                          transform="translate(-521.000000, -1202.000000)"
                          fill="#000000"
                        >
                          <path
                            d="M544.345,1213.39 L534.615,1202.6 C534.167,1202.15 533.57,1201.95 532.984,1201.99 C532.398,1201.95 531.802,1202.15 531.354,1202.6 L521.624,1213.39 C520.797,1214.22 520.797,1215.57 521.624,1216.4 C522.452,1217.23 523.793,1217.23 524.621,1216.4 L532.984,1207.13 L541.349,1216.4 C542.176,1217.23 543.518,1217.23 544.345,1216.4 C545.172,1215.57 545.172,1214.22 544.345,1213.39"
                            id="chevron-up"
                            sketch:type="MSShapeGroup"
                          ></path>
                        </g>
                      </g>
                    </svg>
                  </a>
                </div>
              </th>
              <th scope="col" class="px-6 py-4 whitespace-nowrap">Alpha-2 code</th>
              <th scope="col" class="px-6 py-4 whitespace-nowrap">Alpha-3 code</th>
              <th scope="col" class="px-6 py-4 whitespace-nowrap">Native Name</th>
              <th scope="col" class="px-6 py-4 whitespace-nowrap">Alternative Name</th>
              <th scope="col" class="px-6 py-4 whitespace-nowrap">Calling Code</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="country in paginateCountries"
              :key="country.cca2"
              class="odd:bg-white odd:dark:bg-gray-900 even:bg-gray-50 even:dark:bg-gray-800 border-b dark:border-gray-700"
            >
              <td
                scope="row"
                class="sticky left-0 z-10 bg-inherit px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white"
              >
                <img :src="country.flags.png" :alt="country.name.official" class="w-8" />
              </td>
              <td class="px-6 py-4">
                <a
                  href=""
                  class="hover:underline hover:text-blue-500 transition duration-200"
                  @click.prevent="handleCountryClick(country)"
                  >{{ country.name.official }}</a
                >
              </td>
              <td class="px-6 py-4">{{ country.cca2 }}</td>
              <td class="px-6 py-4">{{ country.cca3 }}</td>
              <td class="px-6 py-4">{{ country.name.nativeName }}</td>
              <td class="px-6 py-4">{{ country.altSpellings }}</td>
              <td class="px-6 py-4">{{ country.idd }}</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div v-if="paginateTotal > paginatePerPage">
        <Pagination v-model="paginatePage" :total="paginateTotal" :per-page="paginatePerPage" />
      </div>
    </div>
  </div>

  <ModalCountry v-model="isModalCountryOpen" :country="selectedCountry" />
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import Fuse from 'fuse.js'

import Pagination from '@/components/Pagination.vue'
import ModalCountry from '@/components/ModalCountry.vue'
import type { Country } from '../types'

const isModalCountryOpen = ref(false)
const selectedCountry = ref<Country>()
const countries = ref<Country[]>([])
const searchTerm = ref('')
const sortKey = ref('')
const sortDirection = ref('')
const fuse = ref<Fuse<Country>>()
const paginatePage = ref(1)
const paginatePerPage = ref(25)
const paginateTotal = computed(() => filteredCountries.value.length)
const paginateCountries = computed(() => {
  const start = (paginatePage.value - 1) * paginatePerPage.value
  const end = start + paginatePerPage.value
  return filteredCountries.value.slice(start, end)
})
const filteredCountries = computed(() => {
  // all countries
  let data = countries.value

  // search term and fule exists
  if (searchTerm.value && fuse.value) {
    const fuseResults = fuse.value.search(searchTerm.value)

    if (sortDirection.value) {
      if (sortDirection.value === 'asc') {
        data = fuseResults
          .sort((a, b) => (a.item.name.official > b.item.name.official ? 1 : -1))
          .map((result) => result.item)
      } else {
        data = fuseResults
          .sort((a, b) => (a.item.name.official < b.item.name.official ? 1 : -1))
          .map((result) => result.item)
      }
    } else {
      data = fuseResults.map((result) => result.item)
    }
  } else if (sortDirection.value) {
    if (sortDirection.value === 'asc') {
      data = data.sort((a, b) => (a.name.official > b.name.official ? 1 : -1))
    } else {
      data = data.sort((a, b) => (a.name.official < b.name.official ? 1 : -1))
    }
  }

  return data
})

/**
 * Handle sort
 */
const handleSort = () => {
  sortKey.value = 'name.official'
  sortDirection.value = sortDirection.value === 'asc' ? 'desc' : 'asc'
}

/**
 * Handle on search input
 */
const handleSearchInput = () => {
  paginatePage.value = 1
}

/**
 * Get countries.
 */
const getCountries = () => {
  fetch('https://restcountries.com/v3.1/all')
    .then((response) => response.json())
    .then((data) => {
      countries.value = data

      fuse.value = new Fuse(countries.value, { keys: ['name.official'] })
    })
    .catch((error) => {
      console.error('Error fetching data:', error)
    })
}

/**
 * Handle country click to detail
 *
 * @param {Country} country
 */
const handleCountryClick = (country: Country) => {
  isModalCountryOpen.value = true
  selectedCountry.value = country
}

/**
 * Will be called after the component has been mounted.
 */
onMounted(() => {
  getCountries()
})
</script>

<style scoped></style>
