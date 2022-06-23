<template>
	<input type="checkbox" v-for="chk in checkList" :key="chk.id" :id="chk.id" :value="chk.value" v-model="checked"
		@change="changeEvent(chk)" />
	{{ checked }}
	{{ listData }}
</template>

<script>
import { ref } from 'vue';

export default {
	props: {
		listData: Array,
	},
	emits: ['ttoto'],
	setup(props, { emit }){
		const checkList = [
			{
				id: 'a',
				value: 'a'
			},
			{
				id: 'b',
				value: 'b'
			},
			{
				id: 'c',
				value: 'c'
			},
		];
		let checked = ref([])

		const changeEvent = (chk) => {
			emit('ttoto', chk);
		}

		return {
			checkList,
			checked,
			changeEvent
		}
	}
}
</script>
