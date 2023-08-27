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
</script>

<template>
  <div>
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
            {{ column.title }}
          </header>

          <draggable
            v-model="column.tasks"
            group="tasks"
            handle=".drag-handler"
            item-key="id"
            :animation="150"
          >
            <template #item="{element: task}: {element: Task}">
              <TrelloBoardTask
                :task="task"
              />
            </template>
          </draggable>

          <footer>
            <button class="text-gray-500">
              + Add a Card
            </button>
          </footer>
        </div>
      </template>
    </draggable>
  </div>
</template>
