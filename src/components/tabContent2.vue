<template>
	<div style="background:red">
		{{ text }}
		<ContentContent :f="func" :t="text" />
	</div>
</template>

<script>
import ContentContent from './ContentContent.vue';

export default {
	components: {
		ContentContent,
	},
	props: {
		text: Array,
		func: Function,
	},
}
</script>
