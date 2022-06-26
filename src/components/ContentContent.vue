<template>
	<div style="background:yellow">
		{{t}}
		<button type="button" @click="f">xxxxxx</button>
	</div>
</template>

<script>
export default {
	props: {
		t: Array,
		f: Function,
	},
}
</script>
