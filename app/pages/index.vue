<script setup lang="ts">

useHead({
  title: 'Список продуктів'
})

import { h, resolveComponent } from 'vue'
import { getPaginationRowModel } from '@tanstack/vue-table'
import type { TableColumn } from '@nuxt/ui'

const table = useTemplateRef('table')

const UBadge = resolveComponent('UBadge')
const UButton = resolveComponent('UButton')


type Product = {
  title: string
  description: string
  price: number
  rating: number
  brand: string
  category: string
  thumbnail: string
}

const { data, status } = await useFetch<{ products: any[] }>(
    'https://dummyjson.com/products',
    {
      key: 'table-products',
      lazy: true
    }
)

const products = computed<Product[]>(() => {
  return (data.value?.products ?? []).map((product) => ({
    title: product.title,
    description: product.description,
    price: product.price,
    rating: product.rating,
    brand: product.brand,
    category: product.category,
    thumbnail: product.thumbnail
  }))
})

const columns: TableColumn<Product>[] = [
  {
    accessorKey: 'title',
    header: 'Назва',
    cell: ({ row }) => {
      return h('span', {
        class: 'font-bold'
      }, row.getValue('title'))
    }
  },
  {
    accessorKey: 'description',
    header: 'Опис',
    cell: ({ row }) => {
      return h('div', {
        class: 'max-w-[85%] whitespace-normal break-words'
      }, row.getValue('description'))
    }
  },
  {
    accessorKey: 'price',
    header: ({ column }) => {
      const isSorted = column.getIsSorted()

      return h(UButton, {
        color: 'neutral',
        variant: 'ghost',
        label: 'Ціна',
        icon: isSorted
            ? isSorted === 'asc'
                ? 'i-lucide-arrow-up-narrow-wide'
                : 'i-lucide-arrow-down-wide-narrow'
            : 'i-lucide-arrow-up-down',
        class: '-mx-2.5',
        onClick: () => column.toggleSorting(column.getIsSorted() === 'asc')
      })
    },
    cell: ({ row }) => {
      const price = row.getValue('price')
      return `$${Number(price)}`
    }
  },
  {
    accessorKey: 'rating',
    header: 'Оцінка',
    cell: ({row}) => {
      const rating = parseFloat(row.getValue('rating'))
      const color = rating < 4.5 ? 'error' : 'success'

      return h(UBadge, {class: 'capitalize', variant: 'subtle', color}, () =>
          rating
      )
    }
  },
  {
    accessorKey: 'brand',
    header: 'Бренд'
  },
  {
    accessorKey: 'category',
    header: 'Категорія'
  },
  {
    accessorKey: 'thumbnail',
    header: 'Фото',
    cell: ({ row }) => {
      const url = row.getValue('thumbnail') as string

      return h('img', {
        src: url,
        alt: 'Фото',
        width: 100,
        height: 100,
        style: 'object-fit: cover; border-radius: 0.5rem;' // для гарного вигляду
      })
    }
  }
]

const globalFilter = ref('')

const pagination = ref({
  pageIndex: 0,
  pageSize: 4
})

</script>

<template>
  <div class="p-6">
    <h2 class="text-2xl font-bold text-green-600 text-center mb-4">
      Список продуктів
    </h2>

    <div class="flex flex-col flex-1 w-full">
      <div class="flex px-4 py-3.5 border-b border-accented">
        <UInput v-model="globalFilter" class="w-sm" placeholder="Фільтр..." />
      </div>

      <UTable
          ref= "table"
          v-model:pagination="pagination"
          v-model:global-filter="globalFilter"
          :data="products"
          :columns="columns"
          :loading="status === 'pending'"
          :pagination-options="{
          getPaginationRowModel: getPaginationRowModel()
          }"
          class="flex-1" />

      <div class="flex justify-center border-t border-default pt-4">
        <UPagination
            :default-page="(table?.tableApi?.getState().pagination.pageIndex || 0) + 1"
            :items-per-page="table?.tableApi?.getState().pagination.pageSize"
            :total="table?.tableApi?.getFilteredRowModel().rows.length"
            @update:page="(p: number) => table?.tableApi?.setPageIndex(p - 1)"
        />
      </div>
    </div>
  </div>
</template>
