<template>
  <TransitionRoot appear :show="modelValue" as="template">
    <Dialog as="div" @close="handleModalClose" class="relative z-10">
      <TransitionChild
        as="template"
        enter="duration-300 ease-out"
        enter-from="opacity-0"
        enter-to="opacity-100"
        leave="duration-200 ease-in"
        leave-from="opacity-100"
        leave-to="opacity-0"
      >
        <div class="fixed inset-0 bg-black/25" />
      </TransitionChild>

      <div class="fixed inset-0 overflow-y-auto">
        <div class="flex min-h-full items-center justify-center p-4 text-center">
          <TransitionChild
            as="template"
            enter="duration-300 ease-out"
            enter-from="opacity-0 scale-95"
            enter-to="opacity-100 scale-100"
            leave="duration-200 ease-in"
            leave-from="opacity-100 scale-100"
            leave-to="opacity-0 scale-95"
          >
            <DialogPanel
              class="w-full max-w-md transform overflow-hidden rounded-xl bg-white p-6 text-left align-middle shadow-xl transition-all"
            >
              <DialogTitle as="h3" class="text-lg font-medium leading-6 text-gray-900">
                Country Detail
              </DialogTitle>

              <a
                href=""
                @click.prevent="handleModalClose"
                class="absolute right-5 top-5 bg-slate-100 hover:bg-slate-500 transition duration-200 rounded-full p-1"
              >
                <svg
                  class="fill-current text-slate-400 hover:text-slate-50 transition duration-200 w-6"
                  viewBox="0 0 32 32"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="M7.004 23.087l7.08-7.081-7.07-7.071L8.929 7.02l7.067 7.069L23.084 7l1.912 1.913-7.089 7.093 7.075 7.077-1.912 1.913-7.074-7.073L8.917 25z"
                  />
                </svg>
              </a>

              <div class="mt-3 text-sm">
                <div>
                  <h2>{{ country?.name.common }}</h2>
                  <div>
                    <p><strong>Official Name:</strong> {{ country?.name.official }}</p>
                    <p><strong>Native Names:</strong></p>
                    <ul>
                      <li
                        v-for="(nativeName, language) in country?.name?.nativeName"
                        :key="language"
                      >
                        {{ nativeName.common }} ({{ language }})
                      </li>
                    </ul>
                  </div>

                  <div>
                    <p><strong>Top-Level Domains (TLD):</strong></p>
                    <ul>
                      <li v-for="tld in country?.tld" :key="tld">{{ tld }}</li>
                    </ul>
                  </div>

                  <div>
                    <p><strong>Country Codes:</strong></p>
                    <ul>
                      <li><strong>Alpha-2 Code:</strong> {{ country?.cca2 }}</li>
                      <li><strong>Numeric Code:</strong> {{ country?.ccn3 }}</li>
                      <li><strong>Alpha-3 Code:</strong> {{ country?.cca3 }}</li>
                      <li><strong>IOC Code:</strong> {{ country?.cioc }}</li>
                    </ul>
                  </div>

                  <div>
                    <p><strong>Independence:</strong> {{ country?.independent ? 'Yes' : 'No' }}</p>
                    <p><strong>Status:</strong> {{ country?.status }}</p>
                    <p><strong>UN Member:</strong> {{ country?.unMember ? 'Yes' : 'No' }}</p>
                  </div>

                  <div>
                    <p><strong>Currencies:</strong></p>
                    <ul>
                      <li v-for="(currency, code) in country?.currencies" :key="code">
                        {{ currency.name }} ({{ code }}) - Symbol: {{ currency.symbol }}
                      </li>
                    </ul>
                  </div>

                  <div>
                    <p><strong>International Dialing Code:</strong></p>
                    <p>Root: {{ country?.idd.root }}</p>
                    <p>Suffixes: {{ country?.idd.suffixes.join(', ') }}</p>
                  </div>

                  <div>
                    <p><strong>Capital:</strong></p>
                    <p>{{ country?.capital.join(', ') }}</p>
                  </div>

                  <div>
                    <p><strong>Alternate Spellings:</strong></p>
                    <ul>
                      <li v-for="spelling in country?.altSpellings" :key="spelling">
                        {{ spelling }}
                      </li>
                    </ul>
                  </div>

                  <p><strong>Region:</strong> {{ country?.region }}</p>

                  <p><strong>Subregion:</strong> {{ country?.subregion }}</p>

                  <p>
                    <strong>Languages:</strong>
                    {{ country?.languages ? Object.values(country?.languages).join(', ') : '' }}
                  </p>

                  <div>
                    <strong>Translations:</strong>
                    <ul>
                      <li v-for="(translation, lang) in country?.translations" :key="lang">
                        <strong>{{ lang }}:</strong> {{ translation.common }}
                      </li>
                    </ul>
                  </div>

                  <div>
                    <strong>Latitude and Longitude:</strong> {{ country?.latlng.join(', ') }}
                  </div>

                  <div><strong>Landlocked:</strong> {{ country?.landlocked ? 'Yes' : 'No' }}</div>

                  <div>
                    <strong>Borders:</strong>
                    <ul>
                      <li v-for="border in country?.borders" :key="border">{{ border }}</li>
                    </ul>
                  </div>

                  <div><strong>Area:</strong> {{ country?.area }} square kilometers</div>

                  <div>
                    <strong>Demonyms:</strong> {{ country?.demonyms.eng.m }} (female),
                    {{ country?.demonyms.eng.f }} (male)
                  </div>

                  <div>
                    <strong>Flag:</strong>
                    {{ country?.flag }}
                  </div>

                  <div>
                    <strong>Google Maps:</strong>
                    <a :href="country?.maps.googleMaps" target="_blank" rel="noopener noreferrer"
                      >Open Google Maps</a
                    >
                  </div>

                  <div>
                    <strong>OpenStreetMaps:</strong>
                    <a
                      :href="country?.maps.openStreetMaps"
                      target="_blank"
                      rel="noopener noreferrer"
                      >Open OpenStreetMaps</a
                    >
                  </div>

                  <div><strong>Population:</strong> {{ country?.population.toLocaleString() }}</div>

                  <div><strong>FIFA Code:</strong> {{ country?.fifa }}</div>

                  <div>
                    <strong>Car:</strong> {{ country?.car.signs[0] }} (Drive on the
                    {{ country?.car.side }} side)
                  </div>

                  <div><strong>Timezones:</strong> {{ country?.timezones.join(', ') }}</div>

                  <div><strong>Continents:</strong> {{ country?.continents.join(', ') }}</div>

                  <div>
                    <strong>Flag:</strong>
                    <img :src="country?.flags.svg" alt="Country Flag" class="w-10" />
                  </div>

                  <div>
                    <strong>Coat of Arms:</strong>
                    <img :src="country?.coatOfArms.svg" alt="Coat of Arms" class="w-10" />
                  </div>

                  <div>
                    <strong>Start of Week:</strong>
                    {{ country?.startOfWeek }}
                  </div>

                  <div>
                    <strong>Capital Info:</strong>
                    <div>Latitude: {{ country?.capitalInfo.latlng[0] }}</div>
                    <div>Longitude: {{ country?.capitalInfo.latlng[1] }}</div>
                  </div>
                </div>
              </div>
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>

<script setup lang="ts">
import type { Country } from '@/types'
import { TransitionRoot, TransitionChild, Dialog, DialogPanel, DialogTitle } from '@headlessui/vue'

interface IProps {
  modelValue: boolean
  country?: Country
}

const props = defineProps<IProps>()
const emits = defineEmits(['update:modelValue'])

/**
 * Handle modal close
 */
const handleModalClose = () => {
  emits('update:modelValue', false)
}
</script>
