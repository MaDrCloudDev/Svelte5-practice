<script lang="ts">
	import TasksForm from '$lib/TasksForm.svelte';
	import TasksList from '$lib/TasksList.svelte';

	let currentFilter = $state<Filter>('all');
	let tasks = $state<Task[]>([]);
	let tasksDone = $derived(tasks.reduce((total, task) => total + Number(task.done), 0));

	let filteredTasks = $derived.by(() => {
		switch (currentFilter) {
			case 'all': {
				return tasks;
			}
			case 'done': {
				return tasks.filter((task) => task.done);
			}
			case 'todo': {
				return tasks.filter((task) => !task.done);
			}
		}
	});

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

	function removeTask(id: string) {
		const index = tasks.findIndex((task) => task.id === id);
		tasks.splice(index, 1);
	}
</script>

{#snippet filterButton(filter: Filter)}
	<button
		onclick={() => (currentFilter = filter)}
		class="secondary filterButton"
		class:contrast={currentFilter === filter}>{filter}</button
	>
{/snippet}

<main>
	<h1>Tasks App</h1>
	<TasksForm {addTask} />
	<p>
		{#if tasks.length !== 0}
			{tasksDone}/{tasks.length} tasks completed.
		{:else}
			Add a task!
		{/if}
	</p>
	{#if tasks.length !== 0}
		<div class="filter">
			{@render filterButton('all')}
			{@render filterButton('todo')}
			{@render filterButton('done')}
		</div>
	{/if}
	<TasksList tasks={filteredTasks} {toggleDone} {removeTask} />
</main>

<style>
	.filterButton {
		text-transform: capitalize;
	}
	.filter {
		display: flex;
		justify-content: end;
		margin-bottom: 1rem;
		gap: 0.5rem;
	}
	main {
		margin: 1rem auto;
		max-width: 35rem;
	}
</style>
