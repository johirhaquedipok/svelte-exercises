<script lang="ts">
  import Navbar from "../components/navbar.svelte";
  import Sidebar from "../components/sidebar.svelte";


	type Todo = {
		id: number;
		text: string,
		status: boolean
	}
	let todos = $state<Todo[]>([]);

	let inputError = $state<string>("")

	function addTodo (event:KeyboardEvent) {
		if(event.key !== 'Enter') return;

		
		const todoEl = event.target as HTMLInputElement;
		const text = todoEl.value;

	if(!text) {
		inputError = 'Input field can not be empty';
		return;
	}

		todos = [...todos, {text, id:todos.length + 1, done: false}];
		todoEl.value = "";
		inputError = "";
	}
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
			
					<input onkeydown={addTodo} type="text" class="w-full py-3 pl-10 pr-4 text-gray-700 bg-white border rounded-md dark:bg-gray-900 dark:text-gray-300 dark:border-gray-600 focus:border-blue-400 dark:focus:border-blue-300 focus:outline-none focus:ring focus:ring-blue-300 focus:ring-opacity-40" placeholder="Input your text">

					
				</div>
				<!-- empty nput field error  -->
				{#if inputError}
				<p class="text-red-500">{inputError}</p>
				{/if}

			{#each todos as {text,id, status}}
				<div   class=" inset-x-0 px-6 py-3 mx-5 mt-4 overflow-y-auto bg-white border rounded-md max-h-72 dark:bg-gray-900 dark:border-gray-700">
					<a href={id.toString()} class="block py-1" >
						<h3 class="font-medium text-gray-700 dark:text-gray-100 hover:underline">{text}</h3>
						<p class="mt-2 text-sm text-gray-500 dark:text-gray-400">Status: {status ? "done" : "pending"}</p>
					</a>
				</div>
				{/each}
			</section>
		</div>

	</div>
	</div>