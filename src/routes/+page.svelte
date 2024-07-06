<script lang="ts">

	type Todo = {
		text: string;
		done: boolean;
	}

	type Filters = 'all' | 'active' | 'completed'

	let todos = $state<Todo[]>([])
	let filter = $state<Filters>('all')

	let filteredTodos = $derived(filterTodos())

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

	function toggleTodo(event:Event) {
		const inputEl = event.target as HTMLInputElement;
		const index = inputEl.dataset.index;
		todos[index].done = !todos[index].done;
	}

	function setFilter(newFilter:Filters) {
		filter = newFilter
	}


	function filterTodos() {
		switch (filter) {
			case 'all':
				
				return todos;
				case 'active':
				
				return todos?.filter(todo => !todo?.done)
				case 'completed':
				
				return todos?.filter(todo => !todo?.done)
		
			default:
				return todos;
		}
	}

	function remaining () {
		return todos.filter(todo => !todo?.done)?.length
	}

	$effect(() =>{
		console.log(todos)
	})
</script>


<input onkeydown={addTodo} type="text" placeholder="Add todo" />
{#each filteredTodos as todo, i}
	<div>
		<input oninput={editTodo} data-index = {i} type='text' value={todo.text}>
		<input onchange={toggleTodo} data-index = {i} type='checkbox' value={todo.done} >
	</div>

{/each}


<div>
	{#each ['all', 'active', 'completed'] as filterItem, i}
	<button onclick={() => setFilter(filterItem)}>
	{filterItem}
	</button>
	{/each}
</div>

{remaining()}