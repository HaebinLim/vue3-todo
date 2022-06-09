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
    <form @submit.prevent="onSubmit">
      <!-- 
      @submit.prevent
      e.preventDefault(); 
      submit될때 새로고침 방지 -->
      <div class="d-flex">
        <div class="flex-grow-1">
          <input 
            type="text" 
            class="form-control" 
            v-model="todo" 
            placeholder="Type new to-do" 
          />
        </div>
        <button type="submit" class="btn btn-primary">
          Add
        </button>
      </div>
      <div v-show="hasError" style="color:red">
        This field cannot be empty
      </div>
    </form>
    <div v-if="!todos.length">추가된 Todo가 없습니다.</div>
    <div 
      v-for="(todo, index) in todos" 
      :key="todo.id" 
      class="card mt-2"
    >
      <div class="card-body p-2 d-flex align-items-center">
        <div class="form-check flex-grow-1">
          <input 
            :id="`ID${todo.id}`" 
            type="checkbox" 
            class="form-check-input" 
            v-model="todo.completed" 
          />
          <label 
            :for="`ID${todo.id}`" 
            class="form-check-label" 
            :class="{ done: todo.completed }"
            :style="todo.completed ? todoStyle : {}"
          >
            {{ todo.subject }}
          </label>
        </div>
        <div>
          <button 
            class="btn btn-danger btn-sm"
            @click="deleteTodo(index)"
          >
            Delete
          </button>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  setup(){

    const toggle = ref(false);
    const onToggle = () => {
      toggle.value = !toggle.value;
    }

    const todo = ref('');
    const todos = ref([]);
    const hasError = ref(false);

    const onSubmit = () => {
      if (todo.value === '') {
        hasError.value = true;
      } else {
        todos.value.push({
          id: Date.now(), // 거의 유니크
          subject: todo.value,
          completed: false,
        });
        hasError.value = false;
        todo.value = '';
      }
    }

    const todoStyle = {
      textDecoration: 'line-through',
      color: 'gray',
    }

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    }

    return {
      toggle,
      onToggle,
      todo,
      todos,
      hasError,
      onSubmit,
      todoStyle,
      deleteTodo,
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