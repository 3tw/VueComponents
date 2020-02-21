# VueComponents
VueJs compontents I use in my projects

## PageTransitions.vue

Insert inside App.vue

```
<page-transition >
	<router-view />
</page-transition>
```

## LanguageChart.vue 
### * still in progress *
Language chart to display the percentage of languages used in a project by making requests to GitHub API repo with axios.

First, install Axios
```
npm install --save axios
```

Add it to main.js

```
import Vue from 'vue'
import App from './App'
import axios from 'axios'

Vue.prototype.$http = axios
```

Pass the url of repository and the languages used in it to the parent data 
```
data() {
	return {
		usedLanguages: {
			html: "HTML",
			css: "CSS",
			javaScript: "JavaScript",
			shell: "Shell"
		},	
		repoUrl: "https://api.github.com/repos/<USER>/<REPOSITORY>languages",
	}
}	
```

If you wish to  display  different combinations of languages in the same projects you must:

1) Add all languages to the LanguageChart.vue data.
```
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
		}
	
	etc.
```

2) Specify languages for each parent component. Let's say you want to use HTML, JavaScript and CSS.
It is important to set the values exactly as they are named in 

``` https://api.github.com/repos/<USER>/<REPOSITORY>languages ```

```
data() {
	return {
		usedLanguages: {
			html: "HTML",
			css: "CSS",
			javaScript: "JavaScript",
		}
	}
}	
```

3) Add desired languages to the template of the LangugesChart.vue.

```
<template>
	<div class="language-chart">
		<div class="language-bar">
			<span v-bind:style="htmlBar" v-if="htmlBar.width !== null">html</span>
			<span v-bind:style="cssBar" v-if="cssBar.width !== null">css</span>
			<span v-bind:style="javascriptBar" v-if="javascriptBar.width !== null">js</span>
		</div>
	</div>
</template>
```
