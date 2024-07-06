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
		const savedTodos = localStorage.getItem('todos');
		if (savedTodos) {
			todos = JSON.parse(savedTodos);
		}
	})

	$effect(() => {
		localStorage.setItem('todos', JSON.stringify(todos))
	})
</script>


<input onkeydown={addTodo} type="text" placeholder="Add todo" class="block w-full py-2.5 text-gray-700 placeholder-gray-400/70 bg-white border border-gray-200 rounded-lg pl-5 pr-11 rtl:pr-5 rtl:pl-11 dark:bg-gray-900 dark:text-gray-300 dark:border-gray-600 focus:border-blue-400 dark:focus:border-blue-300 focus:ring-blue-300 focus:outline-none focus:ring focus:ring-opacity-40 mb-4" />


<!-- <fieldset class="space-y-3">
    <legend class="text-base font-semibold text-slate-900 dark:text-slate-200">Today</legend>
    <div class="grid grid-cols-[1fr_24px] items-center gap-6">
      <label class="peer grid grid-cols-[auto_1fr] items-center gap-3 rounded-md px-2 hover:bg-slate-100">
        <input type="checkbox" class="peer size-3.5 appearance-none rounded-sm border border-slate-300 accent-pink-500 dark:accent-pink-600 checked:appearance-auto" checked="">
        <span class="select-none text-slate-700 peer-checked:text-slate-400 peer-checked:line-through">Create a to do list</span>
      </label>
      <div class="size-[26px] rounded-md p-1 hover:bg-red-50 hover:text-red-500 peer-has-[:checked]:hidden">
        <svg fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12"></path>
        </svg>
      </div>
    </div>
    <div class="grid grid-cols-[1fr_24px] items-center gap-6">
      <label class="peer grid grid-cols-[auto_1fr] items-center gap-3 rounded-md px-2 hover:bg-slate-100">
        <input type="checkbox" class="peer size-3.5 appearance-none rounded-sm border border-slate-300 accent-pink-500 dark:accent-pink-600 checked:appearance-auto">
        <span class="select-none text-slate-700 peer-checked:text-slate-400 peer-checked:line-through">Check off first item</span>
      </label>
      <div class="size-[26px] rounded-md p-1 hover:bg-red-50 hover:text-red-500 peer-has-[:checked]:hidden">
        <svg fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12"></path>
        </svg>
      </div>
    </div>
    <div class="grid grid-cols-[1fr_24px] items-center gap-6">
      <label class="peer grid grid-cols-[auto_1fr] items-center gap-3 rounded-md px-2 hover:bg-slate-100">
        <input type="checkbox" class="peer size-3.5 appearance-none rounded-sm border border-slate-300 accent-pink-500 dark:accent-pink-600 checked:appearance-auto">
        <span class="select-none text-slate-700 peer-checked:text-slate-400 peer-checked:line-through">Investigate race condition</span>
      </label>
      <div class="size-[26px] rounded-md p-1 hover:bg-red-50 hover:text-red-500 block peer-has-[:checked]:hidden">
        <svg fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12"></path>
        </svg>
      </div>
    </div>
  </fieldset> -->
<div class="space-y-4">

	{#each filteredTodos as todo, i}
	<div class="bg-slate-200 ">
		<input oninput={editTodo} data-index = {i} type='text' value={todo.text} class="block w-full py-2.5 text-gray-700 placeholder-gray-400/70 bg-white border border-gray-200 rounded-lg pl-5 pr-11 rtl:pr-5 rtl:pl-11 dark:bg-gray-900 dark:text-gray-300 dark:border-gray-600 focus:border-blue-400 dark:focus:border-blue-300 focus:ring-blue-300 focus:outline-none focus:ring focus:ring-opacity-40 relative">
		
		<input onchange={toggleTodo} data-index = {i} type='checkbox' value={todo.done} class="absolute right-0 focus:outline-none rtl:left-0 rtl:right-auto">
	</div>
	
	{/each}
</div>
	

	<div class="space-x-2">
	{#each ['all', 'active', 'completed'] as filterItem, i}
	<button onclick={() => setFilter(filterItem)} class="px-6 py-2 font-medium tracking-wide text-white capitalize transition-colors duration-300 transform bg-blue-600 rounded-lg hover:bg-blue-500 focus:outline-none focus:ring focus:ring-blue-300 focus:ring-opacity-80">
	{filterItem}
	</button>
	{/each}
</div>

{remaining()}