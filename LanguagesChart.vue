<template>
	<div class="language-chart">
		<div class="language-bar">
			<span v-bind:style="vueBar" v-if="vueBar.width !== null" >random</span>
			<span v-bind:style="htmlBar" v-if="htmlBar.width !== null" >html</span>
			<span v-bind:style="cssBar" v-if="cssBar.width !== null">css</span>
			<span v-bind:style="jsBar" v-if="jsBar.width !== null">js</span>
			<span v-bind:style="shellBar" v-if="shellBar.width !== null">shell</span>
		</div>
		<div class="language-keys"></div>
	</div>
</template>

<script>
export default {
	name: "LanguageChart",
	props: {
		repoUrl: String,
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
			jsBar: {
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
			languages: null,
		};
	},
	mounted () {
		const axios = require("axios")
		axios
			.get(this.repoUrl)
			// use arrow functions - they don't have their own "this"
			.then(response => {
				this.languages = response.data;
				this.updateLanguages()
			})
			.catch(error => {
				console.log(error);
			})
			.then(() => {});
	},	
	methods: {
		updateLanguages: function () {
			let sum = 0;
			let html = this.languages.HTML;
			let css = this.languages.CSS;
			let js = this.languages.JavaScript;
			let shell = this.languages.Shell;
			//let vue = this.lanugages.VueJs

			let languagesArray = [
				html, 
				css,
				js,
				shell,
				//vue
			];

			// count only present languages
			for (let i = 0; i < languagesArray.length; i++) {
				if (languagesArray[i] != undefined) {
					sum += languagesArray[i]
				}
			}

			let htmlWidth = (100 * html) / sum;
			let cssWidth = (100 * css) / sum;
			let jsWidth = (100 * js) / sum;
			let shellWidth = (100 * shell) / sum;
			//let vueWidth = (100 * vue) / sum

			this.$set(this.htmlBar, "width", htmlWidth + "%");
			this.$set(this.cssBar, "width", cssWidth + "%");
			this.$set(this.jsBar, "width", jsWidth + "%");
			this.$set(this.shellBar, "width", shellWidth + "%");
			//this.$set(this.vueBar, "width", vueWidth + "%");
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