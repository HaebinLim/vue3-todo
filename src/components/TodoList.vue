<template>
	<div v-for="(todo, index) in todos" :key="todo.id" class="card mt-2">
		<div class="card-body p-2 d-flex align-items-center">
			<div class="form-check flex-grow-1">
				<input 
					:id="`ID${todo.id}`" 
					type="checkbox" 
					class="form-check-input" 
					:checked="todo.completed" 
					@change="toggleTodo(index)"
				/>
				<!-- props는 one way down bindinig
					v-model 은 양방향 바인딩이어서 @change로 바꿈 -->
				<label :for="`ID${todo.id}`" class="form-check-label" :class="{ done: todo.completed }"
					:style="todo.completed ? todoStyle : {}">
					{{ todo.subject }}
				</label>
			</div>
			<div>
				<button class="btn btn-danger btn-sm" @click="deleteTodo(index)">
					Delete
				</button>
			</div>
		</div>
	</div>
</template>

<script>
import { watchEffect } from 'vue';
export default {
	props: {
		todos: {
			type: Array,
			required: true,
		}
	},
	emits: ['toggle-todo', 'delete-todo'],
	setup(props, { emit }) {
		watchEffect(() => {
			console.log(props.todos.length);
		});
		const toggleTodo = (index) => {
			emit('toggle-todo', index);
		}
		const deleteTodo = (index) => {
			emit('delete-todo', index);
		}
		return {
			toggleTodo,
			deleteTodo
		}
	}
}
</script>

<style>

</style>