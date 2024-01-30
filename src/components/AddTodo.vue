<script setup>
import { ref } from 'vue';

const today = new Date();
const minDate = ref('');
const text = ref('');
const duedate = ref('');
const emit = defineEmits(['todoSubmitted']);

const year = today.getFullYear();
let month = (today.getMonth() + 1).toString().padStart(2, '0');
let day = today.getDate().toString().padStart(2, '0');
minDate.value = `${year}-${month}-${day}`;

const formatDate = (tgl) => {

  // Define arrays for short weekday and month names
  const weekdays = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
  const months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];

  // Get individual date components
  const dayOfWeek = weekdays[tgl.getDay()];
  const month = months[tgl.getMonth()];
  const dayOfMonth = tgl.getDate();
  const year = tgl.getFullYear();

  // Create the formatted date string
  const formattedDate = `${dayOfWeek}, ${month} ${dayOfMonth} ${year}`;
  return formattedDate
};

const onSubmit = () => {
  if (!text.value || !duedate.value) {
    // Display a toast error message if either field is empty
    alert('Please input task and due date')
    return;
  }
  const currentDate = new Date();
  const dueDate = new Date(duedate.value);

  const current = formatDate(currentDate);
  const due = formatDate(dueDate);

  const todoData = {
    name: text.value,
    current: current,
    due: due
  };

  emit('todoSubmitted', todoData);

  // Clear form fields
  text.value = '';
  duedate.value = ''
};
</script>

<template>

<div class="card pb-2">
    <div class="card-body">
        <div class="d-flex flex-row align-items-center">
            
            <div class="input-group">
              <input type="text" class="form-control form-control-lg" placeholder="Add new..." v-model="text">
              <span class="input-group-text">Due Date :</span>
              <input type="date" :min="minDate" class="form-control form-control-lg me-3" v-model="duedate">
            </div>
            <button type="button" class="btn btn-primary" @click="onSubmit">Add</button>
        </div>
    </div>
</div>

</template>

