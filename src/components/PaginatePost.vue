<script setup>
const props = defineProps(['inicio', 'fin', 'postLength']);
const emit = defineEmits(['ePrev', 'eNext']);
const anterior = () => {
	console.log('Previous page...');
	emit('ePrev');
}
</script>

<template>
	<div class="btn-group" role="group" aria-label="Basic example">
		<button @click="anterior" :disabled="inicio <= 0" type="button" class="btn btn-sm btn-outline-info">Previous, {{ inicio }}</button>
		<button @click="emit('eNext')" :disabled="fin >= postLength" type="button" class="btn btn-sm btn-outline-info">Next, {{ fin }}</button>
	</div>
</template>

<style></style>