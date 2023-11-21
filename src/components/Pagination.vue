<template>
  <div class="cmp-pagination flex justify-center">
    <vue-awesome-paginate
      :total-items="total"
      v-model="currentPage"
      :items-per-page="perPage"
      :max-pages-shown="3"
      :on-click="handlePageClick"
    >
      <template #prev-button>
        <span>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="white"
            width="12"
            height="12"
            viewBox="0 0 24 24"
          >
            <path d="M8.122 24l-4.122-4 8-8-8-8 4.122-4 11.878 12z" />
          </svg>
        </span>
      </template>

      <template #next-button>
        <span>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="white"
            width="12"
            height="12"
            viewBox="0 0 24 24"
          >
            <path d="M8.122 24l-4.122-4 8-8-8-8 4.122-4 11.878 12z" />
          </svg>
        </span>
      </template>
    </vue-awesome-paginate>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const emits = defineEmits(['onPage', 'update:modelValue'])
const props = defineProps(['modelValue', 'total', 'perPage'])

const handlePageClick = (page: number) => {
  emits('onPage', page)
}

const currentPage = computed({
  get() {
    return props.modelValue
  },
  set(value) {
    emits('update:modelValue', value)
  }
})
</script>

<style lang="scss">
.cmp-pagination {
  .pagination-container {
    column-gap: 10px;
    align-items: center;
  }

  .paginate-buttons {
    height: 35px;
    width: 35px;
    cursor: pointer;
    border-radius: 4px;
    background-color: transparent;
    border: none;
    color: black;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .back-button,
  .next-button {
    background-color: black;
    color: white;
  }

  .active-page {
    background-color: #e5e5e5;
  }

  .paginate-buttons:hover {
    background-color: #f5f5f5;
  }

  .active-page:hover {
    background-color: #e5e5e5;
  }

  .back-button svg {
    transform: rotate(180deg);
  }

  .back-button:hover,
  .next-button:hover {
    background-color: rgb(45, 45, 45);
  }

  .back-button:active,
  .next-button:active {
    background-color: rgb(85, 85, 85);
  }
}
</style>
