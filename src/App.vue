<template>
	<div id="app">
		<LessonContainer />
	</div>
</template>

<script>
	import LessonContainer from "./components/LessonContainer.vue";

	export default {
		name: "App",
		components: {
			LessonContainer
		}
	};
</script>

<style lang="scss">
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		width: 800px;
		padding: 3em 1.2em;
		background: #e6e7e8;
		margin: 60px auto 0 auto;
	}
</style>
