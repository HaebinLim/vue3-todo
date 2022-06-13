<template>
	<form @submit.prevent="onSubmit">
		<!-- 
      @submit.prevent
      e.preventDefault(); 
      submit될때 새로고침 방지 -->
		<div class="d-flex">
			<div class="flex-grow-1">
				<input type="text" class="form-control" v-model="todo" placeholder="Type new to-do" />
			</div>
			<button type="submit" class="btn btn-primary">
				Add
			</button>
		</div>
		<div v-show="hasError" style="color:red">
			This field cannot be empty
		</div>
	</form>
</template>

<script>
import { ref } from 'vue';
export default {
	emits: ['add-todo'],
	setup(props, { emit }) {
		const todo = ref('');
		const hasError = ref(false);
		const onSubmit = () => {
			if (todo.value === '') {
				hasError.value = true;
			} else {
				emit('add-todo', {
					id: Date.now(), // 거의 유니크
					subject: todo.value,
					completed: false,
				});
				hasError.value = false;
				todo.value = '';
			}
		}
		return {
			todo,
			hasError,
			onSubmit
		}
	},
}
</script>
