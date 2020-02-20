<template>
	<div class="language-chart">
		<div class="language-bar">
			<span v-bind:style="vueBar">vue</span>
			<span v-bind:style="htmlBar">html</span>
			<span v-bind:style="cssBar">css</span>
		</div>
		<div class="language-keys"></div>
	</div>
</template>

<script>
export default {
	name: "LanguageChart",
	data() {
		return {
			vueBar: {
				color: "green",
				backgroundColor: "green",
				lineHeight: "10px",
				fontSize: "10px"
			},
			htmlBar: {
				color: "yellow",
				backgroundColor: "yellow",
				lineHeight: "10px",
				fontSize: "10px"
			},
			cssBar: {
				color: "pink",
				backgroundColor: "pink",
				lineHeight: "10px",
				fontSize: "10px"
			}
		};
	},
	mounted() {
		const axios = require("axios");
		const t3vLanguages = "https://api.github.com/repos/3tw/T3V/languages";
		const climateLanguages = "https://api.github.com/repos/3tw/ClimatePrototype/languages";
		const TomLanguages = "https://api.github.com/repos/3tw/TomWebsite/languages";

		const requestOne = axios.get(t3vLanguages);
		const requestTwo = axios.get(climateLanguages);
		const requestThree = axios.get(TomLanguages);

		axios
			.all([requestOne, requestTwo, requestThree])
			.then(
				axios.spread((...responses) => {
					const responseOne = responses[0];
					const responseTwo = responses[1];
					const responesThree = responses[2];

					console.log(responseOne.data, responseTwo.data, responesThree.data);
				})
			)
			.catch(error => {
				console.log(error);
			})
			.then(() => {});
	},
	created() {}
};
</script>

<style lang="sass" scoped>
.language-bar
    width: 100%
    overflow: hidden
    display: flex

</style>