<template>
  <main>
	<article class="container">
		<v2SuggestionArea v-model="testo" :options="suggestions" ref="suggestion"/>
	</article>
	<p>{{ testo }}</p>
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
			text: "",
			testo: ""
		}
	},
	mounted() {
		this.suggestions.push('@Test1');
		this.suggestions.push('@Test2');
		this.suggestions.push('@Test3');
		this.suggestions.push('@Test4');
	},
	methods: {
		seeValue() {
			let value = this.$refs.suggestion.currentText;
			this.text = value;
		}
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
