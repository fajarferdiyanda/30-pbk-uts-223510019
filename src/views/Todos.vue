<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a,b) =>{
	return a.createdAt - b.createdAt
}))

watch(name, (newVal) => {
	localStorage.setItem('name', newVal)
})

watch(todos, (newVal) => {
	localStorage.setItem('todos', JSON.stringify(newVal))
}, {
	deep: true
})

const addTodo = () => {
	if (input_content.value.trim() === '' || input_category.value === null) {
		return
	}

	todos.value.push({
		content: input_content.value,
		category: input_category.value,
		done: false,
		editable: false,
		createdAt: new Date().getTime()
	})
}

const removeTodo = (todo) => {
	todos.value = todos.value.filter((t) => t !== todo)
}

onMounted(() => {
	name.value = localStorage.getItem('name') || ''
	todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>


<script>
export default {
  props: {
    todos: {
      type: Array,
      required: true
    }
  }
};
</script>

<template>
	<main class="app">
		
		<section class="greeting">
			<h2 class="title">
				What's up, <input type="text" id="name" placeholder="Name here" v-model="name">
			</h2>
		</section>

		<section class="create-todo">
			<h3>List Kegiatan Hari ini</h3>

			<form id="new-todo-form" @submit.prevent="addTodo">
				<h4>What's on your todo list?</h4>
				<input 
					type="text" 
					name="content" 
					id="content" 
					placeholder="e.g. make a video"
					v-model="input_content" />
				
				<h4>Pick a category</h4>
				<div class="options">

					<label>
    <input 
        type="radio" 
        name="category" 
        id="category3" 
        value="health"
        v-model="input_category" />
    <span class="bubble health"></span>
    <div>Health</div>
</label>

<label>
    <input 
        type="radio" 
        name="category" 
        id="category4" 
        value="education"
        v-model="input_category" />
    <span class="bubble education"></span>
    <div>Education</div>
</label>

<label>
    <input 
        type="radio" 
        name="category" 
        id="category5" 
        value="entertainment"
        v-model="input_category" />
    <span class="bubble entertainment"></span>
    <div>Entertainment</div>
</label>

<label>
    <input 
        type="radio" 
        name="category" 
        id="category6" 
        value="other"
        v-model="input_category" />
    <span class="bubble other"></span>
    <div>Other</div>
</label>

				</div>

				<input type="submit" value="Add todo" />
			</form>
		</section>

		<section class="todo-list">
			<h3>TODO LIST</h3>
			<div class="list" id="todo-list">

				<div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
					<label>
						<input type="checkbox" v-model="todo.done" />
						<span :class="`bubble ${
							todo.category == 'business' 
								? 'business' 
								: 'personal'
						}`"></span>
					</label>

					<div class="todo-content">
						<input type="text" v-model="todo.content" />
					</div>

					<div class="actions">
						<button class="delete" @click="removeTodo(todo)">Delete</button>
					</div>
				</div>

			</div>
		</section>

	</main>
</template>

<style>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: aqua;
}

.greeting {
  margin-bottom: 20px;
}

.title {
  font-size: 20px;
}

#create-todo {
  margin-bottom: 20px;
}

.create-todo,
.todo-list {
  width: 100%;
  max-width: 500px;
}

.options {
  display: flex;
  justify-content: space-around;
  margin-bottom: 10px;
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.todo-item.done {
  background-color: #d4edda;
}

.bubble {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  display: inline-block;
  margin-right: 10px;
}

.bubble.personal {
  background-color: #007bff;
}

.bubble.college {
  background-color: #ffc107;
}
</style>
