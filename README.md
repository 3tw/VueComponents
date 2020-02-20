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

Add/remove spans and data according to languages used in the project.

Pass the url of repository to the LanguageChart component 
```
<template>
	<LanguageChart :repoUrl='"https://api.github.com/repos/<USER>/<REPOSITORY>languages"/>
</template>
```



