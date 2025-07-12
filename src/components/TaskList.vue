<template>
	<div class="task-list">
		<h2>Minhas Tarefas</h2>

		<NewTaskForm @create-task="addTask" />

		<ul>
			<li v-for="(task, index) in tasks" :key="index" :class="{ done: task.done }" @click="toggleTask(index)">
				{{ task.title }}
			</li>
		</ul>
	</div>
</template>

<script setup>
import { ref } from "vue";
import NewTaskForm from "./NewTaskForm.vue";

const tasks = ref([
	{ title: "Estudar Vue.js", done: false },
	{ title: "Criar backend em Go", done: true },
	{ title: "Conectar frontend com API", done: false },
]);

function addTask(task) {
	tasks.value.push(task);
}

function toggleTask(index) {
	tasks.value[index].done = !tasks.value[index].done;
}
</script>

<style scoped>
.task-list {
	margin: 20px;
}
.done {
	text-decoration: line-through;
	color: gray;
}
li {
	cursor: pointer;
	padding: 6px;
	transition: background 0.3s;
}
li:hover {
	background: #f0f0f0;
}
</style>
