<script lang="ts">
	import TasksForm from '$lib/TasksForm.svelte';
	import TasksList from '$lib/TasksList.svelte';

	let tasks = $state<Task[]>([]);
	let tasksDone = $derived(tasks.reduce((total, task) => total + Number(task.done), 0));

	function addTask(newTask: string) {
		tasks.push({
			id: crypto.randomUUID(),
			title: newTask,
			done: false
		});
	}

	function toggleDone(task: Task) {
		task.done = !task.done;
	}

	function removeTask(index: number) {
		tasks.splice(index, 1);
	}
</script>

<main>
	<h1>Tasks App</h1>
	<TasksForm {addTask} />
	{#if tasks.length !== 0}
		<p>{tasksDone}/{tasks.length} tasks completed.</p>
	{:else}
		<p>Add a task!</p>
	{/if}
	<TasksList {tasks} {toggleDone} {removeTask} />
</main>

<style>
	main {
		margin: 1rem auto;
		max-width: 35rem;
	}
</style>
