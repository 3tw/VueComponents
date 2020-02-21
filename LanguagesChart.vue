<template>
	<div class="language-chart">
		<div class="language-bar">
			<span v-bind:style="htmlBar" v-if="htmlBar.width !== null">html</span>
			<span v-bind:style="cssBar" v-if="cssBar.width !== null">css</span>
			<span v-bind:style="javascriptBar" v-if="javascriptBar.width !== null">js</span>
			<span v-bind:style="shellBar" v-if="shellBar.width !== null">shell</span>
		</div>
		<div class="language-keys"></div>
	</div>
</template>

<script>
export default {
	name: "LanguageChart",
	props: {
		repoUrl: {
			type: String,
			required: true,
		},
		usedLanguages: {
			type: Object,
			required: true,
		},
	},
	data() {
		return {
			vueBar: {
				color: "#01FF70",
				backgroundColor: "#01FF70",
				lineHeight: "10px",
				fontSize: "10px",
				width: null
			},
			htmlBar: {
				color: "#7FDBFF",
				backgroundColor: "#7FDBFF",
				lineHeight: "10px",
				fontSize: "10px",
				width: null
			},
			cssBar: {
				color: "#F012BE",
				backgroundColor: "#F012BE",
				lineHeight: "10px",
				fontSize: "10px",
				width: null
			},
			javascriptBar: {
				color: "#FFDC00",
				backgroundColor: "#FFDC00",
				lineHeight: "10px",
				fontSize: "10p",
				width: null
			},
			shellBar: {
				color: "#DDDDDD",
				backgroundColor: "#DDDDDD",
				lineHeight: "10px",
				fontSize: "10px",
				width: null
			},
			languages: null
		};
	},
	mounted () {
		// make axios request
		const axios = require("axios")
		axios
			.get(this.repoUrl)
			// use arrow functions - they don't have their own "this"
			.then(response => {
				this.languages = response.data;
				//this.updateArray()
				this.updateLanguages()
			})
			.catch(error => {
				console.log(error);
			})
			.then(() => {});

		
	},	
	methods: {
		updateLanguages: function () {
			//let selectedLanguages = this.usedLanguages;
			let fetchedLanguages = this.languages;
			let languagesValues = Object.values(this.usedLanguages);
			//let languagesKeys = Object.keys(this.usedLanguages);
			let sum = 0;

			for (let i = 0; i < languagesValues.length; i++) {
				let item = languagesValues[i];
				sum += fetchedLanguages[item];	
			}

			class Language {
				constructor (fetchedName){
					this.fetchedName = fetchedName
				}
				percentage() {
					let valueName = this.fetchedName;
					let value = fetchedLanguages[valueName];
					let percentage = (100 * value) / sum;
					let rounded = Math.round( percentage * 10 ) / 10;
					return rounded + "%"
				}
			}
			
			for (let i = 0; i < languagesValues.length; i++) {
				let fetchedName = languagesValues[i];
				let dataObject = `this.${fetchedName.toLowerCase()}Bar`;
				let instance = new Language (fetchedName);
				this.$set(eval(dataObject), "width", instance.percentage());
			}
		},
	},
};
</script>

<style lang="sass" scoped>
.language-bar
    width: 100%
    overflow: hidden
    display: flex

</style>