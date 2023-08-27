<script setup lang="ts">
import draggable from 'vuedraggable'
import { nanoid } from 'nanoid'
import { Column, Task } from '~/types'
import DragHandle from '~/components/DragHandle.vue'

const columns = ref<Column[]>([
  {
    id: nanoid(),
    title: 'Backlog',
    tasks: [
      {
        title: 'Create marketing landing page',
        createdAt: new Date(),
        id: nanoid()
      },
      {
        title: 'Develop cool new feature',
        createdAt: new Date(),
        id: nanoid()
      },
      { title: 'Fix page nav bug', createdAt: new Date(), id: nanoid() }
    ]
  },
  { title: 'Selected for Dev', id: nanoid(), tasks: [] },
  { title: 'In Progress', id: nanoid(), tasks: [] },
  { title: 'QA', id: nanoid(), tasks: [] },
  { title: 'Complete', id: nanoid(), tasks: [] }
])

const createColumn = () => {
  const column: Column = {
    id: nanoid(),
    title: '',
    tasks: []
  }

  columns.value.push(column)

  nextTick(() => {
    const input = document.querySelector('.column:last-of-type .title-input') as HTMLInputElement
    input.focus()
  })
}

</script>

<template>
  <div class="flex items-start overflow-x-auto gap-4 ">
    <draggable
      v-model="columns"
      group="columns"
      item-key="id"
      class="flex gap-4 items-start"
      :animation="150"
      handle=".drag-handler"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-200 p-5 rounded min-w-[250px]">
          <header class="font-bold mb-4">
            <DragHandle />
            <input
              v-model="column.title"
              class="title-input bg-transparent focus:bg-white rounded px-1 w-4/5"
              type="text"
              @keyup.enter="($event.target as HTMLInputElement).blur()"
              @keydown.delete="column.title === '' ? columns = columns.filter(c => c.id !== column.id) : null"
            >
          </header>

          <draggable
            v-model="column.tasks"
            group="tasks"
            handle=".drag-handler"
            item-key="id"
            :animation="150"
          >
            <template #item="{element: task}: {element: Task}">
              <div>
                <TrelloBoardTask
                  :task="task"
                  @delete="column.tasks = column.tasks.filter(t => t.id !== $event)"
                />
              </div>
            </template>
          </draggable>

          <footer>
            <NewTask @add="column.tasks.push($event)" />
          </footer>
        </div>
      </template>
    </draggable>
    <button
      class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-50"
      @click="createColumn"
    >
      + Add Another Column
    </button>
  </div>
</template>
