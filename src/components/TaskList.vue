<template>
	<div class="task-list">
		<h2>Minhas Tarefas</h2>

		<NewTaskForm @create-task="addTask" />

		<!-- Filters -->
		<div class="filters">
			<button
				v-for="option in filterOptions"
				:key="option"
				:class="{ active: currentFilter === option }"
				@click="currentFilter = option"
			>
				{{ option }}
			</button>
		</div>

		<!-- Filtered list -->
		<ul>
			<li
				v-for="(task, index) in filteredTasks"
				:key="index"
				:class="{ done: task.done }"
				@click="toggleTask(index)"
			>
				{{ task.title }}
			</li>
		</ul>
	</div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from "vue";
import NewTaskForm from "./NewTaskForm.vue";

const STORAGE_KEY = "vue-tasks";

const tasks = ref([]);

// Load saved tasks
onMounted(() => {
	const saved = localStorage.getItem(STORAGE_KEY);
	if (saved) {
		tasks.value = JSON.parse(saved);
	}
});

// Save tasks whenever they change
watch(
	tasks,
	(newTasks) => {
		localStorage.setItem(STORAGE_KEY, JSON.stringify(newTasks));
	},
	{ deep: true },
);

const filterOptions = ["Todas", "Pendentes", "Concluídas"];
const currentFilter = ref("Todas");

// Computed: filter tasks based on the active filter
const filteredTasks = computed(() => {
	if (currentFilter.value === "Pendentes") {
		return tasks.value.filter((t) => !t.done);
	} else if (currentFilter.value === "Concluídas") {
		return tasks.value.filter((t) => t.done);
	}
	return tasks.value;
});

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
.filters {
	margin-bottom: 1rem;
}
.filters button {
	margin: 0 5px;
	padding: 6px 12px;
	background-color: #e0e0e0;
	border: none;
	cursor: pointer;
}
.filters button.active {
	background-color: #42b983;
	color: white;
}
</style>
