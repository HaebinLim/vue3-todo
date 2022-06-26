<template>

  <router-view></router-view>
  <TestWrap />
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
</template>

<script>
import { ref, computed, watchEffect, reactive } from 'vue';
import TodoSimpleForm from '@/components/TodoSimpleForm.vue';
import TodoList from '@/components/TodoList.vue';
import TestWrap from '@/components/TestWrap.vue';

export default {
  components: {
    TodoSimpleForm,
    TodoList,
    TestWrap
  },
  setup(){
    const toggle = ref(false);
    const onToggle = () => {
      toggle.value = !toggle.value;
    }

    const a = reactive({
      b: 1,
      c: 3,
    });
    // 리액티브한 state 값이 바뀔 때마다 실행
    watchEffect(() => {
      console.log('a', a.b);
    });
    /*
    특정 데이터가 변경되었을 때 실행, 현재 데이터와 이전 데이터를 가져옴
    watch(() => [a.b, a.c], (current, prev) => {
      console.log('x', current, prev);
    });
    a.b = 4;
    
    watch([currentPage, totalNum], (currentPage, prev)=> {
      console.log(currentPage, prev);
    })
    */


    const count = ref(1);
    const doubleCountComputed = computed(() => {
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

    /*
    const filterTodos = computed(()=>{
      if (searchText.value) {
        return todos.value.filter(val => {
          return val.subject.includes(searchText.value);
        });
      }
      return todos.value;
    })*/


    return {
      toggle,
      onToggle,
      count,
      doubleCountComputed,
      doubleCountMethod
    }
  }
}
</script>