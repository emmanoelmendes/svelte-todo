<script>

import { afterUpdate } from 'svelte';

afterUpdate(() => {
	document.querySelector('.js-todo-input').focus();
});

let todoItems = [];
let newTodo = '';

function addTodo() {
	newTodo = newTodo.trim();
	if (!newTodo) return;

	const todo = {
		text: newTodo,
		checked: false,
		id: Date.now(),
	};

	todoItems = [...todoItems, todo];
	newTodo = '';
}

function toggleDone(id) {
	const index = todoItems.findIndex(item => item.id === Number(id));
	todoItems[index].checked = !todoItems[index].checked;
}

function deleteTodo(id) {
	todoItems = todoItems.filter(item => item.id !== id);
}

</script>

<main>
	<div class="container">
		<h1 class="app-title">Todos</h1>

		<ul class="todo-list">
			{#each todoItems as todo (todo.id)}
				<li class="todo-item {todo.checked ? 'done' : ''}">
					<input on:click={() => toggleDone(todo.id)} type="checkbox" id={todo.id} />
					<label for={todo.id} class="tick"></label>
					<span>{todo.text}</span>
					<button on:click={() => deleteTodo(todo.id)} class="delete-todo">
						<svg><use href="#delete-icon"></use></svg>
					</button>
				</li>
			{/each}
		</ul>

		<div class="empty-state">
			<svg class="checklist-icon"><use href="#checklist-icon"></use></svg>
			<h2 class="empty-state__title">Add your first todo</h2>
			<p class="empty-state__description">What do you want to get done today?</p>
		</div>

		<form on:submit|preventDefault={addTodo}>
			<input 
				type="text" 
				bind:value={newTodo}
				class="js-todo-input"
				placeholder="E.g. Build a web app" 
				arial-label="Enter a new todo item" 
			/>
		</form>
	</div>
</main>
