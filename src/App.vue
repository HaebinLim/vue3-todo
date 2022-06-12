<template>
  <!-- display: none/block 
        초기 렌더링 비용이 많이 듬 (토글하여 자주 바뀔 때 사용) -->
  <div v-show="toggle">true</div>
  <div v-show="!toggle">false</div>

  <!-- 조건에 맞는 div만 렌더링 
        토글하는데 비용이 많이 듬 (런타임 중 잘 안바뀔 때 사용 -->
  <div v-if="toggle">true</div>
  <div v-else>false</div>
  <button type="button" @click="onToggle">Toggle</button>

  <div class="container">
    <h2>To-Do List</h2>
    
    <TodoSimpleForm @add-todo="addTodo" />

    <div v-if="!todos.length">추가된 Todo가 없습니다.</div>
    
    <TodoList :todos="todos" />
  </div>
</template>

<script>
import { ref } from 'vue';
import TodoSimpleForm from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';

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

    const addTodo = (todo) => {
      todos.value.push(todo);
    }

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    }

    const todoStyle = {
      textDecoration: 'line-through',
      color: 'gray',
    }

    return {
      toggle,
      onToggle,
      todos,
      addTodo,
      deleteTodo,
      todoStyle,
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