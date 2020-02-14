<template>
	<div>
		<div class="container">
			<img
				class="conductor-img"
				width="75"
				height="75"
				:src="require('@/assets/conductor.png')"
				alt=""
			/>
			<h1 class="recital-title">{{ recitalTitle }}</h1>
			<h2 class="instrument-title">{{ instrumentTitle }}</h2>
		</div>
		<LessonDetails v-bind:details="lessonDetailsData" />
	</div>
</template>

<script>
	import axios from "axios";
	import LessonDetails from "./LessonDetails";
	export default {
		name: "LessonContainer",
		components: {
			LessonDetails
		},
		data() {
			return {
				recitalTitle: null,
				instrumentTitle: null,
				lessonDetailsData: [],
				isData: false
			};
		},
		// props: {},
		mounted() {
			axios.get("https://api.myjson.com/bins/qubzl").then(res => {
				this.lessonDetailsData = res.data.lessonDetails;
				this.recitalTitle = res.data.recitalTitle;
				this.instrumentTitle = res.data.instrumentTitle;
				if (this.lessonDetailsData) {
					this.isData = true;
				}
			});
		}
	};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	$header-title-color: #ba0d5c;

	h1,
	h2 {
		color: $header-title-color;
	}
	.conductor-img {
		float: left;
	}
	.instrument-title {
		margin-top: -2.5em;
	}
	.recital-title {
		margin-top: -2em;
	}
	.container {
		display: flex;
		margin-bottom: -4em;
		align-items: center;
		justify-content: space-between;
	}
</style>
