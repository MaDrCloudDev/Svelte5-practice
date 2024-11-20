<script lang="ts">
	let newTask = $state('');
	let {
		addTask
	}: {
		addTask: (newTask: string) => void;
	} = $props();

	function formSubmitted(e: SubmitEvent) {
		e.preventDefault();
		addTask(newTask);
		newTask = '';
	}
</script>

<form onsubmit={formSubmitted}>
	<label>
		Task
		<input name="newTask" bind:value={newTask} />
	</label>
	<div>
		<button class="submit-button">Add</button>
	</div>
</form>

<style>
	.submit-button {
		margin-left: 1rem;
		margin-top: 0.3rem;
	}
	label {
		width: 100%;
	}
	form {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
</style>
