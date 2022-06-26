<template>
	<div style="font-size:30px">{{ tabIndex }}</div>
	<button type="button" @click="tabIndex = 0">버튼1</button>
	<button type="button" @click="tabIndex = 1">버튼2</button>
	<tabContent1 v-if="tabIndex === 0" :text="list" :func="a" ref="table" />
	<tabContent2 v-if="tabIndex === 1" :text="list" :func="b" ref="table" />

	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
</template>

<script>
import { ref } from 'vue';

import tabContent1 from './tabContent1.vue';
import tabContent2 from './tabContent2.vue';

let header = '';

export default {
	components: {
		tabContent1,
		tabContent2
	},
	setup(){
		const list = ref([]);
		const tabIndex = ref(0);
		const table = ref(null);
		list.value = [{ id: 123, name: 'sdfsdf' }];
		const a = () => {
			header = '우앙';
			console.log('탭 인덱스'+ tabIndex.value);
		}

		return {
			list,
			tabIndex,
			table,
			a,
		}
	},
	methods: {
		b(){
			header = '우앙';
			console.log('우앙', header);
		}
	}
}
</script>
