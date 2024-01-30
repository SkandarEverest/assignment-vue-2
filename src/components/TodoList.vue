<script setup>

import { defineProps } from 'vue';

const props = defineProps({
  todos: {
    type: Array,
    required: true,
  },
});

const emit = defineEmits(['todoCompleted','todoDeleted']);

const completeTodo = (checked, id) => {
  emit('todoCompleted', (checked,id));
};

const deleteTodo = (id) => {
  emit('todoDeleted', id);
};

</script>

<template>

    <div id="list-todo">
      <ul 
        class="list-group list-group-horizontal rounded-0 bg-transparent m-2"
        v-for="todo in todos"
        :key="todo.id"
      >

        <li class="list-group-item d-flex align-items-center ps-0 pe-3 py-1 rounded-0 border-0 bg-transparent">
            <div class="form-check">
                <input class="form-check-input me-0" type="checkbox" value="" @change="completeTodo(todo.checked,todo.id)" v-model="todo.checked">
            </div>
        </li>
        <li class="list-group-item px-3 py-1 d-flex align-items-center flex-grow-1 border-0 bg-transparent">
            <p class="lead fw-normal mb-0" :style="{ 'text-decoration': todo.checked ? 'line-through' : '' }">{{ todo.name }}</p>
        </li>
        <li class="list-group-item px-3 py-1 d-flex align-items-center border-0 bg-transparent">
          <div class="px-2 py-2 bg-warning bg-opacity-10 border border-warning rounded">
            <i class="bi bi-hourglass-split text-warning"></i><span style="padding-left: 10px;">{{ todo.duedate }}</span>
          </div>
        </li>
        <li class="list-group-item px-3 py-1 d-flex align-items-center border-0 bg-transparent">
          <i class="bi bi-info-circle-fill"></i><span class="align-middle text-decoration-underline" style="padding-left: 10px; color: darkblue;">{{ todo.createddate }}</span>
        </li>
        <li class="list-group-item px-3 py-1 d-flex align-items-center border-0 bg-transparent">
            <button type="button" class="btn btn-danger" @click="deleteTodo(todo.id)">Delete</button>
        </li>
      </ul>
    </div>

</template>

