<template>
	<InnerTest :listData="list" @ttoto="a" />
</template>

<script>
import { ref } from 'vue';
import InnerTest from './InnerTest.vue';

export default {
	components: {
		InnerTest,
	},
	setup(){
		const list = ref([]);
		list.value = [{ id: 123, name: 'sdfsdf' }, { id: 123, name: 'sdfsdf' }, { id: 123, name: 'sdfsdf' }];

		const a = (res) => {
			console.log(res);
		}
		return {
			list,
			a,
		}
	}
}
</script>
