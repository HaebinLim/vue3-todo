<template>
	<div style="background:pink">
		{{ text }}
	</div>
</template>

<script>
export default {
	props: {
		text: Array,
		func: Function,
	},
	setup(props) {
		props.func();
	}
}
</script>
