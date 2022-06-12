<template>
	<div v-for="(todo, index) in todos" :key="todo.id" class="card mt-2">
		<div class="card-body p-2 d-flex align-items-center">
			<div class="form-check flex-grow-1">
				<input :id="`ID${todo.id}`" type="checkbox" class="form-check-input" v-model="todo.completed" />
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
export default {
	props: {
		todos: {
			type: Array,
			required: true,
		}
	}
}
</script>

<style>

</style>