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

type Payment = {
  id: string
  date: string
  status: 'paid' | 'failed' | 'refunded'
  email: string
  amount: number
}

const data = ref<Payment[]>([
  {
    id: '4600',
    date: '2024-03-11T15:30:00',
    status: 'paid',
    email: 'james.anderson@example.com',
    amount: 594
  },
  {
    id: '4599',
    date: '2024-03-11T10:10:00',
    status: 'failed',
    email: 'mia.white@example.com',
    amount: 276
  },
  {
    id: '4598',
    date: '2024-03-11T08:50:00',
    status: 'refunded',
    email: 'william.brown@example.com',
    amount: 315
  },
  {
    id: '4597',
    date: '2024-03-10T19:45:00',
    status: 'paid',
    email: 'emma.davis@example.com',
    amount: 529
  },
  {
    id: '4596',
    date: '2024-03-10T15:55:00',
    status: 'paid',
    email: 'ethan.harris@example.com',
    amount: 639
  },
  {
    id: '4595',
    date: '2024-03-10T13:20:00',
    status: 'paid',
    email: 'sophia.miller@example.com',
    amount: 428
  },
  {
    id: '4594',
    date: '2024-03-10T11:05:00',
    status: 'paid',
    email: 'noah.wilson@example.com',
    amount: 673
  },
  {
    id: '4593',
    date: '2024-03-09T22:15:00',
    status: 'paid',
    email: 'olivia.jones@example.com',
    amount: 382
  },
  {
    id: '4592',
    date: '2024-03-09T20:30:00',
    status: 'paid',
    email: 'liam.taylor@example.com',
    amount: 547
  },
  {
    id: '4591',
    date: '2024-03-09T18:45:00',
    status: 'paid',
    email: 'ava.thomas@example.com',
    amount: 291
  },
  {
    id: '4590',
    date: '2024-03-09T16:20:00',
    status: 'refunded',
    email: 'lucas.martin@example.com',
    amount: 624
  },
  {
    id: '4589',
    date: '2024-03-09T14:10:00',
    status: 'refunded',
    email: 'isabella.clark@example.com',
    amount: 438
  },
  {
    id: '4588',
    date: '2024-03-09T12:05:00',
    status: 'refunded',
    email: 'mason.rodriguez@example.com',
    amount: 583
  },
  {
    id: '4587',
    date: '2024-03-09T10:30:00',
    status: 'refunded',
    email: 'sophia.lee@example.com',
    amount: 347
  },
  {
    id: '4586',
    date: '2024-03-09T08:15:00',
    status: 'failed',
    email: 'ethan.walker@example.com',
    amount: 692
  },
  {
    id: '4585',
    date: '2024-03-08T23:40:00',
    status: 'failed',
    email: 'amelia.hall@example.com',
    amount: 419
  },
  {
    id: '4584',
    date: '2024-03-08T21:25:00',
    status: 'failed',
    email: 'oliver.young@example.com',
    amount: 563
  },
  {
    id: '4583',
    date: '2024-03-08T19:50:00',
    status: 'refunded',
    email: 'aria.king@example.com',
    amount: 328
  },
  {
    id: '4582',
    date: '2024-03-08T17:35:00',
    status: 'refunded',
    email: 'henry.wright@example.com',
    amount: 647
  },
  {
    id: '4581',
    date: '2024-03-08T15:20:00',
    status: 'failed',
    email: 'luna.lopez@example.com',
    amount: 482
  }
])

const columns: TableColumn<Payment>[] = [
  {
    accessorKey: 'id',
    header: '#',
    cell: ({ row }) => `#${row.getValue('id')}`
  },
  {
    accessorKey: 'date',
    header: 'Date',
    cell: ({ row }) => {
      return new Date(row.getValue('date')).toLocaleString('en-US', {
        day: 'numeric',
        month: 'short',
        hour: '2-digit',
        minute: '2-digit',
        hour12: false
      })
    }
  },
  {
    accessorKey: 'status',
    header: 'Status',
    cell: ({ row }) => {
      const color = {
        paid: 'success' as const,
        failed: 'error' as const,
        refunded: 'neutral' as const
      }[row.getValue('status') as string]

      return h(UBadge, { class: 'capitalize', variant: 'subtle', color }, () =>
          row.getValue('status')
      )
    }
  },
  {
    accessorKey: 'email',
    header: ({ column }) => {
      const isSorted = column.getIsSorted()

      return h(UButton, {
        color: 'neutral',
        variant: 'ghost',
        label: 'Email',
        icon: isSorted
            ? isSorted === 'asc'
                ? 'i-lucide-arrow-up-narrow-wide'
                : 'i-lucide-arrow-down-wide-narrow'
            : 'i-lucide-arrow-up-down',
        class: '-mx-2.5',
        onClick: () => column.toggleSorting(column.getIsSorted() === 'asc')
      })
    }
  },
  {
    accessorKey: 'amount',
    header: () => h('div', { class: 'text-right' }, 'Amount'),
    cell: ({ row }) => {
      const amount = Number.parseFloat(row.getValue('amount'))

      const formatted = new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: 'EUR'
      }).format(amount)

      return h('div', { class: 'text-right font-medium' }, formatted)
    }
  }
]

const sorting = ref([
  {
    id: 'email',
    desc: false
  }
])

const globalFilter = ref('45')

const pagination = ref({
  pageIndex: 0,
  pageSize: 5
})

</script>

<template>

  <UHeading class="text-primary">
    Список продуктів
  </UHeading>

  <div class="flex flex-col flex-1 w-full">
    <div class="flex px-4 py-3.5 border-b border-accented">
      <UInput v-model="globalFilter" class="max-w-sm" placeholder="Filter..." />
    </div>

    <UTable
        ref="table"
        v-model:pagination="pagination"
        v-model:global-filter="globalFilter"
        v-model:sorting="sorting"
        :data="data"
        :columns="columns"
        :pagination-options="{
        getPaginationRowModel: getPaginationRowModel()
      }"
        class="flex-1"
    />

    <div class="flex justify-center border-t border-default pt-4">
      <UPagination
          :default-page="(table?.tableApi?.getState().pagination.pageIndex || 0) + 1"
          :items-per-page="table?.tableApi?.getState().pagination.pageSize"
          :total="table?.tableApi?.getFilteredRowModel().rows.length"
          @update:page="(p: number) => table?.tableApi?.setPageIndex(p - 1)"
      />
    </div>

  </div>

</template>
