<template>
	<div class="container">
		<h2>To-Do List</h2>
		<input type="text" class="form-control" v-model="searchText" placeholder="Search" @keyup.enter="searchTodo" />
		<TodoSimpleForm @add-todo="addTodo" />
		<div style="color:red;"> {{ error }} </div>
		<div v-if="!todos.length">There is noting to display</div>
		<TodoList :todos="todos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" />

		<hr />
		<nav aria-label="Page navigation">
			<ul class="pagination">
				<li class="page-item" v-if="currentPage !== 1">
					<a class="page-link" @click="getTodos(currentPage - 1)">Previous</a>
				</li>
				<li class="page-item" :class="currentPage === page ? 'active' : ''" v-for="page in pages" :key="page">
					<a class="page-link" @click="getTodos(page)">{{ page }}</a>
				</li>
				<li class="page-item" v-if="currentPage !== pages">
					<a class="page-link" @click="getTodos(currentPage + 1)">Next</a>
				</li>
			</ul>
		</nav>

	</div>
</template>


<script>
import { ref, computed, watch } from 'vue';
import TodoSimpleForm from '@/components/TodoSimpleForm.vue';
import TodoList from '@/components/TodoList.vue';
import TestWrap from '@/components/TestWrap.vue';

import axios from 'axios';

export default {
  components: {
    TodoSimpleForm,
    TodoList,
    TestWrap
  },
  setup(){
    const todos = ref([]);
    const searchText = ref('');
    const error = ref('');
    const totalNum = ref(null);
    const limit = 5;
    const currentPage = ref(1);
    const pages = computed(()=>{
      return Math.ceil(totalNum.value/limit);
    })

		const getTodos = async (page = currentPage.value) => {
			currentPage.value = page;
			try {
				const res = await axios.get(`http://localhost:3000/todos?_sort=id&_order=desc&subject_like=${searchText.value}&_page=${page}&_limit=${limit}`);
				totalNum.value = res.headers['x-total-count'];
				todos.value = res.data;
			} catch (err) {
				error.value = 'Something went wrong';
			}
		};
		getTodos();


		const addTodo = async (todo) => {
			error.value = '';
			try {
				await axios.post('http://localhost:3000/todos', {
					// id는 자동으로 추가 됨
					subject: todo.subject,
					completed: todo.completed,
				})
				/*
				.then(res => {
					todos.value.push(res.data);
				}).catch(() => {
					error.value = 'Something went wrong';
				}); */
				getTodos(1);

			} catch (err) {
				error.value = 'Something went wrong';
			}
		}
		const deleteTodo = async (index) => {
			error.value = '';
			const id = todos.value[index].id;
			try {
				await axios.delete('http://localhost:3000/todos/' + id);
				getTodos(1);
			} catch (err) {
				error.value = 'Something went wrong';
			}
		}

		const todoStyle = {
			textDecoration: 'line-through',
			color: 'gray',
		}

		const toggleTodo = async (index) => {
			error.value = '';
			const id = todos.value[index].id;
			try {
				await axios.patch('http://localhost:3000/todos/' + id, {
					completed: !todos.value[index].completed
				});
				todos.value[index].completed = !todos.value[index].completed;
			} catch (err) {
				error.value = 'Something went wrong';
			}
		}
		let timeout = null;
		const searchTodo = () => {
			clearTimeout(timeout);
			getTodos(1);
		}
		watch(searchText, () => {
			clearTimeout(timeout);
			timeout = setTimeout(() => {
				getTodos(1);
			}, 2000)
		})

		return {
			todos,
			addTodo,
			deleteTodo,
			todoStyle,
			toggleTodo,
			searchText,
			//filterTodos,
			error,
			getTodos,
			totalNum,
			currentPage,
			pages,
			searchTodo
		}
	}
}
</script>

<style>
  .done {
    text-Decoration: line-through;
    color: gray;
  }
</style>