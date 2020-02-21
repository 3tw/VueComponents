<template>
	<div class="language-chart">
		<div class="language-bar">
			<span v-bind:style="vueBar" v-if="vueBar.width !== null">.</span>
			<span v-bind:style="htmlBar" v-if="htmlBar.width !== null">.</span>
			<span v-bind:style="cssBar" v-if="cssBar.width !== null">.</span>
			<span v-bind:style="javascriptBar" v-if="javascriptBar.width !== null">.</span>
			<span v-bind:style="shellBar" v-if="shellBar.width !== null">.</span>
		</div>
		<div class="language-keys">
			<span class="vue" v-if="vueBar.width !== null"> Vue {{vueBar.width}} </span>
			<span class="html" v-if="htmlBar.width !== null"> HTML {{htmlBar.width}} </span>
			<span class="css" v-if="cssBar.width !== null"> CSS {{cssBar.width}} </span>
			<span class="js" v-if="javascriptBar.width !== null"> JS {{javascriptBar.width}} </span>
			<span class="shell" v-if="shellBar.width !== null"> Shell {{shellBar.width}} </span>
		</div>
	</div>
</template>

<script>
export default {
	name: "LanguageChart",
	props: {
		repoUrl: {
			type: String,
			required: true
		},
		usedLanguages: {
			type: Object,
			required: true
		}
	},
	data() {
		return {
			vueBar: {
				color: "#345aff",
				backgroundColor: "#346aff",
				lineHeight: "20px",
				fontSize: "1px",
				width: null
			},
			htmlBar: {
				color: "#eafad4",
				backgroundColor: "#eafad4",
				lineHeight: "20px",
				fontSize: "1px",
				width: null
			},
			cssBar: {
				color: "#f5ed5c",
				backgroundColor: "#f5ed5c",
				lineHeight: "20px",
				fontSize: "1px",
				width: null
			},
			javascriptBar: {
				color: "#788DF4",
				backgroundColor: "#788DF4",
				lineHeight: "20px",
				fontSize: "10p",
				width: null
			},
			shellBar: {
				color: "#424242",
				backgroundColor: "#424242",
				lineHeight: "20px",
				fontSize: "1px",
				width: null
			},
			languages: null
		};
	},
	mounted() {
		// make axios request
		const axios = require("axios");
		axios
			.get(this.repoUrl)
			// use arrow functions - they don't have their own "this"
			.then(response => {
				this.languages = response.data;
				this.updateLanguages();
			})
			.catch(error => {
				console.log(error);
			})
			.then(() => {});
	},
	methods: {
		updateLanguages: function() {
			let fetchedLanguages = this.languages;
			let languagesValues = Object.values(this.usedLanguages);
			let sum = 0;

			for (let i = 0; i < languagesValues.length; i++) {
				let item = languagesValues[i];
				sum += fetchedLanguages[item];
			}

			class Language {
				constructor(fetchedName) {
					this.fetchedName = fetchedName;
				}
				percentage() {
					let valueName = this.fetchedName;
					let value = fetchedLanguages[valueName];
					let percentage = (100 * value) / sum;
					let rounded = Math.round(percentage * 10) / 10;
					return rounded + "%";
				}
			}

			for (let i = 0; i < languagesValues.length; i++) {
				let fetchedName = languagesValues[i];
				let dataObject = `this.${fetchedName.toLowerCase()}Bar`;
				let instance = new Language(fetchedName);
				this.$set(eval(dataObject), "width", instance.percentage());
			}
		}
	}
};
</script>

<style lang="sass" scoped>
.language-bar
    width: 100%
    overflow: hidden
    display: flex
.language-keys
	display: flex
	justify-content: flex-start
	margin-top: 0.5rem
	font-weight: 600
	span
		padding: 1px 10px 1px 10px
.html
	background-color: $lime
.vue
	background-color: $blue2
.shell
	background-color: $gray
.js
	background-color: $blue
.css
	background-color: $yellow

</style>