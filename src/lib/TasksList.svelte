<script lang="ts">
	let {
		tasks,
		toggleDone,
		removeTask
	}: {
		tasks: Task[];
		toggleDone: (task: Task) => void;
		removeTask: (index: number) => void;
	} = $props();
</script>

<section>
	{#each tasks as task, index}
		<article class="task">
			<label>
				<input type="checkbox" checked={task.done} onchange={() => toggleDone(task)} />
				<span class:done={task.done}>{task.title}</span>
			</label>
			<button onclick={() => removeTask(index)} class="outline">Remove</button>
		</article>
	{/each}
</section>

<style>
	.task {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	.done {
		text-decoration: line-through;
	}
</style>
