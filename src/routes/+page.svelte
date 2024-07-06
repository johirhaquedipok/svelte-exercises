<script lang="ts">
	let todos = $state([
		{text: 'Todo 1', done: false,},
		{text: 'Todo 2', done : true},
		{text: 'Todo 3', done: false}
	])
</script>


{#each todos as todo, i}
	<div>
		<input type='text' value={todo.text}>
		<input type='checkbox' value={todo.done} >
	</div>

{/each}