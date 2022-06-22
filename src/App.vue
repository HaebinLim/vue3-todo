<template>
  <!-- display: none/block 
        초기 렌더링 비용이 많이 듬 (토글하여 자주 바뀔 때 사용) -->
  <div v-show="toggle">true</div>
  <div v-show="!toggle">false</div>

  <div>count: {{ count }}</div>
  <div>doubleCountComputed: {{ doubleCountComputed }}</div>
  <div>doubleCountComputed: {{ doubleCountComputed }}</div>
  <div>doubleCountMethod: {{ doubleCountMethod() }}</div>
  <div>doubleCountMethod: {{ doubleCountMethod() }}</div>
  <button @click="count++">add one</button>

  <!-- 조건에 맞는 div만 렌더링 
        토글하는데 비용이 많이 듬 (런타임 중 잘 안바뀔 때 사용 -->
  <div v-if="toggle">true</div>
  <div v-else>false</div>
  <button type="button" @click="onToggle">Toggle</button>

  <div class="container">
    <h2>To-Do List</h2>

    <input type="text" class="form-control" v-model="searchText" placeholder="Search" />


    <TodoSimpleForm @add-todo="addTodo" />
    <div style="color:red;"> {{ error }} </div>

    <div v-if="!filterTodos.length">There is noting to display</div>

    <TodoList :todos="filterTodos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" />

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
import { ref, computed } from 'vue';
import TodoSimpleForm from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';
import axios from 'axios';

export default {
  components: {
    TodoSimpleForm,
    TodoList
  },
  setup(){
    const toggle = ref(false);
    const onToggle = () => {
      toggle.value = !toggle.value;
    }

    const todos = ref([]);
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
        const res = await axios.get(`http://localhost:3000/todos?_page=${page}&_limit=${limit}`);
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
        const res = await axios.post('http://localhost:3000/todos', {
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
        todos.value.push(res.data);
      } catch (err) {
        error.value = 'Something went wrong';
      }
    }
    const deleteTodo = async (index) => {
      error.value = '';
      const id = todos.value[index].id;
      try {
        await axios.delete('http://localhost:3000/todos/' + id);
        todos.value.splice(index, 1);
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

    const count = ref(1);
    const doubleCountComputed = computed(()=>{
      // 매개변수를 받아올 수 없음
      // 캐시되어 한번만 실행
      // console.log('computed')
      return count.value * 2;
    })
    const doubleCountMethod = () => {
      // 매개변수 사용가능
      // 호출되는 만큼 실행
      // console.log('method')
      return count.value * 2;
    }

    const searchText = ref('');
    const filterTodos = computed(()=>{
      if (searchText.value) {
        return todos.value.filter(val => {
          return val.subject.includes(searchText.value);
        });
      }
      return todos.value;
    })

    return {
      toggle,
      onToggle,
      todos,
      addTodo,
      deleteTodo,
      todoStyle,
      toggleTodo,
      count,
      doubleCountComputed,
      doubleCountMethod,
      searchText,
      filterTodos,
      error,
      getTodos,
      totalNum,
      currentPage,
      pages
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