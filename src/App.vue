<template>
  <main>
	<article class="container">
		<v2SuggestionArea v-model="text" :options="suggestions" ref="suggestion"/>
	</article>
	<p>{{ text }}</p>
  </main>
</template>

<script>
import v2SuggestionArea from './components/v2-SuggestionArea.vue'

export default {
	components: {
		v2SuggestionArea
	},
	data() {
		return {
			suggestions: [],
			text: ""
		}
	},
	mounted() {
		this.suggestions.push('@Test1');
		this.suggestions.push('@Test2');
		this.suggestions.push('@Test3');
		this.suggestions.push('@Test4');
	}
}
</script>

<style>

input {
	display: block;
	margin-top: 20px;
}
.container {
	display: block;
	margin-top: 300px;
	margin-left: 50px;
}
</style>
