<template>
  <main>
	<article class="container">
		<v2SuggestionArea :options="suggestions" ref="suggestion"/>
	</article>
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
		this.suggestions.push('##Test##');
		this.suggestions.push('##Qualcosa##');
		this.suggestions.push('##Testino##');
		this.suggestions.push('##Mercoledì##');
		this.suggestions.push('##Penelope##');
		this.suggestions.push('##Figata##');
		this.suggestions.push('##Testone##');
		this.suggestions.push('##Giovedì##');
		this.suggestions.push('##Gioco##');
		this.suggestions.push('##Messaggio1##');
		this.suggestions.push('##Messaggio2##');
		this.suggestions.push('##Messaggio3##');
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
	margin-left: 200px;
}
</style>
