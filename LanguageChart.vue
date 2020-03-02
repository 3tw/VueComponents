<template>
	<div class="language-chart">
		<div class="language-bar">
			<span :style="vueBar" v-if="vueBar.width">.</span>
			<span :style="htmlBar" v-if="htmlBar.width">.</span>
			<span :style="cssBar" v-if="cssBar.width">.</span>
			<span :style="javascriptBar" v-if="javascriptBar.width">.</span>
			<span :style="shellBar" v-if="shellBar.width">.</span>
		</div>
		<div class="language-keys">
			<span class="vue" v-if="vueBar.width">vue {{vueBar.width}}</span>
			<span class="html" v-if="htmlBar.width">html {{htmlBar.width}}</span>
			<span class="css" v-if="cssBar.width">css {{cssBar.width}}</span>
			<span class="js" v-if="javascriptBar.width">javascript {{javascriptBar.width}}</span>
			<span class="shell" v-if="shellBar.width">shell {{shellBar.width}}</span>
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
				color: "#32ff76",
				backgroundColor: "#32ff76",
				lineHeight: "20px",
				fontSize: "1px",
				width: undefined
			},
			htmlBar: {
				color: "#eafad4",
				backgroundColor: "#eafad4",
				lineHeight: "20px",
				fontSize: "1px",
				width: undefined
			},
			cssBar: {
				color: "#FFE40A",
				backgroundColor: "#FFE40A",
				lineHeight: "20px",
				fontSize: "1px",
				width: undefined
			},
			javascriptBar: {
				color: "#ffa58f",
				backgroundColor: "#ffa58f",
				lineHeight: "20px",
				fontSize: "10p",
				width: undefined
			},
			shellBar: {
				color: "#c0c0c0",
				backgroundColor: "#c0c0c0",
				lineHeight: "20px",
				fontSize: "1px",
				width: undefined
			},
			languages: undefined,
			usedLanguages: undefined,			
		};
	},
	mounted() {
		// Axios request
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
			let totalPercentage = 0;

			class Language {
				constructor(fetchedName) {
					this.fetchedName = fetchedName;
				}
				percentage() {
					let valueName = this.fetchedName;
					let value = fetchedLanguages[valueName];
					let percentage = (100 * value) / sum;
					let rounded = Math.round(percentage * 10) / 10;
					return rounded;
				}
			}

			// get sum
			for (let i = 0; i < usedLanguages.length; i++) {
				let item = usedLanguages[i];
				sum += fetchedLanguages[item];
			}

			// get and set width for each language
			for (let i = 0; i < usedLanguages.length - 1; i++) {
				let item = usedLanguages[i];
				let dataObject = `this.${item.toLowerCase()}Bar`;
				let instance = new Language(item);
				this.$set(
					eval(dataObject),
					"width",
					instance.percentage() + "%"
				);
				totalPercentage += instance.percentage();
			}

			// compute last language seperately to insure the totalPercentage = 100
			let lastItem = usedLanguages[usedLanguages.length - 1];
			let lastObject = `this.${lastItem.toLowerCase()}Bar`;
			let lastPercentage = Math.round((100 - totalPercentage) * 10) / 10;
			this.$set(eval(lastObject), "width", lastPercentage + "%");
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
	flex: 1 1 auto
.html
	background-color: #EAFAD4
.vue
	background-color: #32ff76
.shell
	background-color: #c0c0c0
.js
	background-color: #788DF4
.css
	background-color: #FFE40A
</style>