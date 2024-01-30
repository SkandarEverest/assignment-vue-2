<script setup>
import TodoList from './components/TodoList.vue';
import AddTodo from './components/AddTodo.vue';

import { ref, onMounted } from 'vue';

const todos = ref([]);

onMounted(() => {
  const savedTodos = JSON.parse(localStorage.getItem('todos'));

  if (savedTodos) {
    todos.value = savedTodos;}
});

// Generate unique ID
const generateUniqueId = () => {
    return Math.floor(Math.random() * 1000000);
};

// Save todos to local storage
const saveTodosToLocalStorage = () => {
  localStorage.setItem('todos', JSON.stringify(todos.value));
};

const handleTodoSubmitted = (todosData) => {
  todos.value.push({
    id: generateUniqueId(),
    name: todosData.name,
    createddate: todosData.current,
    duedate: todosData.due,
    checked: false,
  });

  saveTodosToLocalStorage();
};

const handleTodoCompleted = (checked,id) => {

  todos.value.map(el => {
        if (el.id === id){
            el.checked = checked;
        }
        return el;
    });

  saveTodosToLocalStorage();

};


const handleTodoDeleted = (id) => {
  todos.value = todos.value.filter(
    (todo) => todo.id !== id
  );

  saveTodosToLocalStorage();

};

</script> 

<template>
  <div class="container py-5 h-100">
      <div class="row d-flex justify-content-center align-items-center h-100">
          <div class="card" id="list1" style="border-radius: .75rem; background-color: #eff1f2;">
              <div class="card-body py-4 px-4 px-md-5">
                  
                <p class="h1 text-center mt-3 mb-4 pb-3 text-primary">
                    <u>My Todo-s</u>
                </p>

                <AddTodo @todoSubmitted="handleTodoSubmitted" />

                <hr class="my-4">

                <TodoList
                    :todos="todos"
                    @todoDeleted="handleTodoDeleted"
                    @todoCompleted="handleTodoCompleted"
                />

              </div>
          </div>
      </div>
  </div>
</template>

