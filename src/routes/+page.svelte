<script lang="ts">
  import { onMount } from "svelte";
  import Navbar from "../components/navbar.svelte";
  import Sidebar from "../components/sidebar.svelte";


	type Todo = {
		id: number;
		text: string,
		status: boolean
	}
	let todos = $state<Todo[]>([{
		text:"hello",
		id: 1,
		status:true
	}]);

	let inputError = $state<string>("")
	
	let startFocus: HTMLInputElement ;

	function addTodo (event:KeyboardEvent) {
		if(event.key !== 'Enter') return;

		
		const todoEl = event.target as HTMLInputElement;
		const text = todoEl.value;

	if(!text) {
		inputError = 'Input field can not be empty';
		return;
	}

		todos = [...todos, {text, id:todos.length + 1, status: false}];
		todoEl.value = "";
		inputError = "";
	}

	function editTodo (event:Event) {
		
		const todoEl = event.target as HTMLInputElement;
		const index = todoEl.dataset.index;

		if(!index) return
		 
		const selected = todos[+index];

		if(!todoEl.value) {
			todoEl.value = selected.text
		}

		todos[+index].text = todoEl.value;
		
	}
	 function toggleTodoStatus (event:Event) {

		const todoEl = event.target as HTMLInputElement;
		const index = todoEl.dataset.index;

		if(!index) return	
		 
		todos[+index].status = !todos[+index].status;
		
	}

onMount(() => startFocus.focus())
</script>
<div class=" m-auto max-h-screen h-screen bg-white shadow dark:bg-gray-800 dark:text-white">
	<Navbar />
	<div class="grid grid-cols-12 h-full gap-4">
		<Sidebar />
		<!-- main section starts -->
		<div class="mt-4 col-span-10">
			<section class="relative w-full max-w-md px-5 py-4 mx-auto rounded-md">
				<div class="relative">
					<span class="absolute inset-y-0 left-0 flex items-center pl-3">
						
						<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-5 text-gray-400">
							<path stroke-linecap="round" stroke-linejoin="round" d="M19.5 14.25v-2.625a3.375 3.375 0 0 0-3.375-3.375h-1.5A1.125 1.125 0 0 1 13.5 7.125v-1.5a3.375 3.375 0 0 0-3.375-3.375H8.25m0 12.75h7.5m-7.5 3H12M10.5 2.25H5.625c-.621 0-1.125.504-1.125 1.125v17.25c0 .621.504 1.125 1.125 1.125h12.75c.621 0 1.125-.504 1.125-1.125V11.25a9 9 0 0 0-9-9Z" />
						  </svg>
						  
					</span>
			
					<input onkeydown={addTodo} bind:this={startFocus} type="text" class="w-full py-3 pl-10 pr-4 text-gray-700 bg-white border rounded-md dark:bg-gray-900 dark:text-gray-300 dark:border-gray-600 focus:border-blue-400 dark:focus:border-blue-300 focus:outline-none focus:ring focus:ring-blue-300 focus:ring-opacity-40" placeholder="Input your text">

					
				</div>
				<!-- empty nput field error  -->
				{#if inputError}
				<p class="text-red-500">{inputError}</p>
				{/if}

			{#each todos as {text,id, status}, i}
				<div   class="px-6 py-3  mt-4 overflow-y-auto bg-white border rounded-md max-h-72 dark:bg-gray-900 dark:border-gray-700">
					<div class="flex py-1 gap-2 relative border border-gray-800 rounded-md" >
						<input oninput={editTodo} data-index={i} value={text} type="text" class="flex-1 w-3/4 py-3  px-4 text-gray-700 bg-white rounded-md dark:bg-gray-900 dark:text-gray-300  focus:border-blue-400 dark:focus:border-blue-300 focus:outline-none focus:ring focus:ring-blue-300 focus:ring-opacity-40" placeholder="Input your text">
						<label for="hs-checkbox-on-right" class="flex w-1/13 absolute right-0 py-3 px-4  bg-white  rounded-lg text-sm focus:border-blue-500 focus:ring-blue-500 dark:bg-gray-900  dark:text-neutral-400">
							<span class="text-sm text-gray-500 dark:text-neutral-400 sr-only">Default </span>
							<input onchange={toggleTodoStatus} data-index={i} type="checkbox" class="shrink-0 ms-auto mt-0.5 border-blue-800 rounded text-blue-600 focus:ring-blue-500 disabled:opacity-50 disabled:pointer-events-none dark:bg-neutral-800 dark:border-neutral-700 dark:checked:bg-blue-500 dark:checked:border-blue-500 dark:focus:ring-offset-gray-800 size-5" id="hs-checkbox-on-right" checked={status} >
						  </label>
					</div>
					<p class="mt-2 text-sm text-gray-500 dark:text-gray-400">Status:
						{#if status} 
						 done
						 {:else}
						  pending
						  {/if}

					</p>
				</div>
				{/each}
			</section>
				
			  
				
		</div>
		

	</div>
	</div>