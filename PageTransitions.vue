<template>
	<transition 
    :name="transitionType" 
    :mode="transitionMode" 
    v-on="hooks" 
    v-bind="$attrs">
		<slot></slot>
	</transition>
</template>

<script>
const DEFAULT_TRANSITION_TYPE = "fade";
const DEFAULT_TRANSITION_MODE = "out-in";

export default {
	name: "PageTransition",
	data() {
		return {
			transitionType: DEFAULT_TRANSITION_TYPE,
			transitionMode: DEFAULT_TRANSITION_MODE
		};
	},
	created() {
		// execute before route changes
		this.$router.beforeEach((to, from, next) => {
			let transitionType =
				to.meta.transitionType || from.meta.transitionType;

			if (transitionType === "slide") {
				// detect direction of route change based on path length
				const toDepth = to.path.split("/").length;
				const fromDepth = from.path.split("/").length;
				transitionType =
					toDepth < fromDepth ? "slide-right" : "slide-left";

				this.transitionType = transitionType || DEFAULT_TRANSITION_TYPE;
			} else {
				this.transitionType = DEFAULT_TRANSITION_TYPE;
			}
			next();
		});
	}
};
</script>

<style lang="sass" scoped>

.slide-left-enter-active,
.slide-left-leave-active,
.slide-right-enter-active,
.slide-right-leave-active
  transition-duration: 0.3s
  transition-property: opacity, transform
  transition-timing-function: ease-in-out // equivalent to cubic-bezier(0.42,0,0.58,1)

.slide-left-enter,
.slide-right-leave-active
    opacity: 0
    transform: translate(2.5em, 0) //slide before dissapearing

.slide-left-leave-active,
.slide-right-enter
    opacity: 0
    transform: translate(-2.5em, 0)

.fade-enter-active, 
.fade-leave-active
  transition-duration: 0.5s
  transition-property: opacity
  transition-timing-function: cubic-bezier(0.42,0,0.60,1)

.fade-enter, 
.fade-leave-to
  opacity: 0
</style>

<!-- 
based on 
markus.oberlehner.net/blog/vue-router-page-transitions/ &
router.vuejs.org/guide/advanced/transitions.html#per-route-transition
-->