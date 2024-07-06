<script lang="ts">
	let todos = $state([
		{text: 'Todo 1', done: false,},
		{text: 'Todo 2', done : true},
		{text: 'Todo 3', done: false}
	])

	function addTodo(event:KeyboardEvent) {
		if(event.key !== 'Enter') return ;

		const todoEl = event.target as HTMLInputElement;
		// const id = window.crypto.randomUUID();
		const text = todoEl.value ;
		const done = false;

		todos = [...todos,{text, done}]
		
		todoEl.value = "";
	}


	function editTodo(event:Event) {
		const inputEl = event.target as HTMLInputElement;
		const index = inputEl.dataset.index;
		todos[index].text = inputEl.value;
	}
</script>


<input onkeydown={addTodo} type="text" placeholder="Add todo" />
{#each todos as todo, i}
	<div>
		<input oninput={editTodo} data-index = {i} type='text' value={todo.text}>
		<input type='checkbox' value={todo.done} >
	</div>

{/each}