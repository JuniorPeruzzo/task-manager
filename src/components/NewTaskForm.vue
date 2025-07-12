<template>
	<form @submit.prevent="addTask" class="new-task-form">
		<input v-model="newTitle" type="text" placeholder="Nova tarefa" required />
		<button type="submit">Adicionar</button>
	</form>
</template>

<script setup>
import { ref } from "vue";

const newTitle = ref("");

// Permite emitir eventos para o componente pai
const emit = defineEmits(["create-task"]);

function addTask() {
	if (!newTitle.value.trim()) return;

	// Emite a nova tarefa para o pai
	emit("create-task", {
		title: newTitle.value,
		done: false,
	});

	newTitle.value = "";
}
</script>

<style scoped>
.new-task-form {
	display: flex;
	gap: 8px;
	margin-bottom: 20px;
}

input {
	flex: 1;
	padding: 8px;
}

button {
	padding: 8px 12px;
	background-color: #42b983;
	color: white;
	border: none;
	cursor: pointer;
}

button:hover {
	background-color: #369f70;
}
</style>
