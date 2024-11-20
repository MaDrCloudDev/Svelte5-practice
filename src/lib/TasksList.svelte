<script lang="ts">
	import { fade } from 'svelte/transition';
	let {
		tasks,
		toggleDone,
		removeTask
	}: {
		tasks: Task[];
		toggleDone: (task: Task) => void;
		removeTask: (id: string) => void;
	} = $props();
</script>

<section>
	{#each tasks as task}
		<article class="task" transition:fade>
			<label>
				<input type="checkbox" checked={task.done} onchange={() => toggleDone(task)} />
				<span class:done={task.done}>{task.title}</span>
			</label>
			<button onclick={() => removeTask(task.id)} class="outline">Remove</button>
		</article>
	{/each}
</section>

<style>
	.outline {
		border: 1px solid red;
		color: red;
	}
	.task {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	.done {
		text-decoration: line-through;
	}
</style>
