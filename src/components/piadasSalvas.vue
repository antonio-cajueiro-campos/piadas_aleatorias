<template>
	<div class=lista>
		
	</div>
</template>

<script>
export default {
	props: {
		saved: {
			type: Array, required: false
		}
	}
}
</script>

<style>
.lista {
	height: 100%;
	width: 400px;
	background: rgb(199, 199, 199);
	position: absolute;
	top: 0;
	left: 0;
	z-index: -1;
}
</style>