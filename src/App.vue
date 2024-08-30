<!-- SHORTCUT -> vueinit -->

<script setup>
import { computed, onMounted, ref } from 'vue'
import BlogPost from './components/BlogPost.vue'
import PaginatePost from './components/PaginatePost.vue'
import LoadingSpinner from './components/LoadingSpinner.vue'
// import ButtonCounter from './components/ButtonCounter.vue'

const posts = ref([]);
const postXPage = 10;
const inicio = ref(0);
const fin = ref(postXPage);
const isLoading = ref(true);

/**-----------------------------------------------------
 *  emit an event from de secondary to main component
 *----------------------------------------------------**/
const favorito = ref('');
const cambiarFavorito = (title) => {
	favorito.value = title;
	// @eCambiarFavorito="cambiarFavorito" in button
	// like prop :eCambiarFavorito="cambiarFavorito"
}

const next = () => {
	inicio.value += postXPage
	fin.value += postXPage
}
const prev = () => {
	inicio.value -= postXPage
	fin.value -= postXPage
}

/**--------------------------------------------
 *      Run after our template is rendered! 
 *---------------------------------------------**/
onMounted( async() => {
	try {
		const response = await fetch('https://jsonplaceholder.typicode.com/posts');
		posts.value = await response.json();
	} catch (error) {
		console.log(error);
	} finally {
		setTimeout(() => { isLoading.value = false; }, 700);
	}
});

// fetch('https://jsonplaceholder.typicode.com/posts')
// 	.then(res => res.json())
// 	.then(data => {
// 		posts.value = data;
// 	}).catch((e) => console.log(e))
// 	.finally(() => {
// 		setTimeout(() => { isLoading.value = false; }, 700);
// 	});

/**---------------------------------------------------------------------------------
 *  computed -> every has changes, we rendered
 *  cuando we use computed method, to access to value, we need use the `.value`
 *--------------------------------------------------------------------------------**/
const maxLength = computed(() => posts.value.length)
</script>

<template>
	<LoadingSpinner v-if="isLoading" />
	<div class="container mt-3" v-else>
		<h1>APP</h1>
		<h2>Mis posts favoritos: {{ favorito }}</h2>
		<!-- <ButtonCounter /> -->

		<PaginatePost
			@ePrev="prev"
			@eNext="next"
			:inicio="inicio"
			:fin="fin"
			:postLength="maxLength"
			class="mb-3"
		/>

		<BlogPost
			v-for="post in posts.slice(inicio, fin)" :key="post.id"
			:title="post.title" :id="post.id"
			:body="post.body" class="mb-3" @eCambiarFavorito="cambiarFavorito"
		/>

		<!-- <BlogPost title="Post 2" :id="2" body="Description 2" />
		<BlogPost title="Post 3" :id="3" /> -->
	</div>
</template>












<!-- <script>
export default {
	// todo lo que retornemos en el metodo data será reactivo
	data() {
		return {
			counter: 0,
		}
	},
	methods: {
		increment() {
			this.counter++
		}
	},
}
</script> -->

<!-- <script>
import { ref } from 'vue'

export default {
  setup() {
    const counter = ref(0)
    const increment = () => {
      counter.value++
    }
    return {
      counter,
      increment
    }
  }
}
</script> -->