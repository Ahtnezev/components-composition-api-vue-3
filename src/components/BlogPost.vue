<script setup>
// receive props without callin'
// defineProps(['title', 'id', 'body']) // #1 
// const props = defineProps(['title', 'id', 'body', 'colorText']) // #2
// console.log(props.title);
// #3 with validations
defineProps({
	title: String,
	id: Number,
	body: {
		type: String,
		default: 'No description'
	},
	// this way it is not recommend, emits better way
	// eCambiarFavorito: Function
})
const emit = defineEmits(['eCambiarFavorito'])
// emit
</script>

<template>
	<div class="card bg-dark text-white">
		<div class="card-body">
			<!-- :class="`text-${colorText}`" -->
			<h5 class="card-title">{{ id }} - {{ title }}</h5>
			<p class="card-text">{{ body }}</p>
			<!-- @click="$emit('eCambiarFavorito', title)" #1 -->
			<!-- @click="emit('eCambiarFavorito', title)" #2 -->
			<!-- @click="eCambiarFavorito(title)"  #3 IT'S NOT RECOMMENDED -->
			<button
				@click="emit('eCambiarFavorito', title)"
				class="btn btn-outline-info"
			>
				Favorito
			</button>
		</div>
	</div>
</template>