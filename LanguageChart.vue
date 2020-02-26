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
			<span class="vue" v-if="vueBar.width !== null">vue {{vueBar.width}}</span>
			<span class="html" v-if="htmlBar.width !== null">html {{htmlBar.width}}</span>
			<span class="css" v-if="cssBar.width !== null">css {{cssBar.width}}</span>
			<span class="js" v-if="javascriptBar.width !== null">javascript {{javascriptBar.width}}</span>
			<span class="shell" v-if="shellBar.width !== null">shell {{shellBar.width}}</span>
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
				color: "#FFE40A",
				backgroundColor: "#FFE40A",
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
				color: "#c0c0c0",
				backgroundColor: "#c0c0c0",
				lineHeight: "20px",
				fontSize: "1px",
				width: null
			},
            languages: null,
            usedLanguages: null
		};
	},
	mounted() {
		const axios = require("axios");
		axios
			.get(this.repoUrl)
			.then(response => {
                this.languages = response.data;
                this.usedLanguages = Object.keys(this.languages);
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
			let usedLanguages = this.usedLanguages;
			let sum = 0;

			for (let i = 0; i < usedLanguages.length; i++) {
				let item = usedLanguages[i];
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

			for (let i = 0; i < usedLanguages.length; i++) {
				let item = usedLanguages[i];
				let dataObject = `this.${item.toLowerCase()}Bar`;
				let instance = new Language(item);
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
	font-weight: 600
	
.language-keys span
	padding: 1px 10px 1px 10px
.html
	background-color: #EAFAD4
.vue
	background-color: #346aff
.shell
	background-color: #c0c0c0
.js
	background-color: #788DF4
.css
	background-color: #FFE40A
</style>