<template>
	<div class="details-container">
		<div class="lesson-container">
			<h2 class="lessons">Lessons</h2>
			<div class="lesson-number-container">
				<div
					v-on:click="getClickedLesson(number)"
					v-bind:key="number.id"
					v-bind:class="
						number.id === clickedLessonId ? 'activeLessonNumber' : 'lesson-numbers'
					"
					v-for="number in lessonsList"
				>
					{{ number.lessonTitle.slice(-1) }}
				</div>
			</div>
			<button class="exit-session-btn"><span> Exit Session </span></button>
		</div>

		<section class="objective-body" v-if="isSelectedLessonData">
			<aside class="menu-container">
				<div
					class="objective-container"
					:class="
						objectiveDone && objective.id === clickedObjectiveId
							? 'doneBorder'
							: objectiveNotDoing && objective.id === clickedObjectiveId
							? 'notDoingBorder'
							: objectiveNextClass && objective.id === clickedObjectiveId
							? 'nextClassBorder'
							: ''
					"
					v-for="objective in selectedLesson[0]"
					v-bind:key="objective.id"
					v-on:click="getObjectiveVideo(objective)"
				>
					<span class="objective-title">{{ objective.title }}</span>
					<span class="objective-duration">({{ objective.duration }} mins)</span>
					<a class="check-icon" @click="done(objective.id)"
						><i class="fas fa-check-circle fa-lg"></i
					></a>
					<a class="cross-icon" @click="notDoing()"
						><i class="fas fa-times-circle fa-lg"></i
					></a>
					<a class="edit-icon" @click="nextClass()"><i class="fas fa-edit fa-lg"></i></a>
				</div>
			</aside>
			<main class="video-container">
				<div class="video-list" v-on:click="playVideoInBG($event)">
					<iframe
						allow="fullscreen"
						v-bind:src="selectedObjectiveURL"
						frameborder="0"
					></iframe>
				</div>
			</main>
		</section>
	</div>
</template>

<script>
	export default {
		name: "LessonDetails",
		data() {
			return {
				isSelectedLessonData: false,
				clickedLessonId: "",
				clickedObjectiveId: "",
				selectedLesson: {},
				selectedObjectiveURL: [],
				lessonsList: [],
				objectiveDone: false,
				objectiveNotDoing: false,
				objectiveNextClass: false,
				filteredLessonsList: []
			};
		},

		methods: {
			getClickedLesson(event) {
				const clickedLesson = JSON.parse(JSON.stringify(event));
				this.clickedLessonId = clickedLesson.id;

				this.selectedLesson = this.details
					.filter(item => item.id === clickedLesson.id)
					.map(o =>
						o.objectiveDetails.map(objective => {
							return {
								title: objective.title,
								duration: objective.durationInMinutes,
								id: objective.id,
								objectiveVideosDetails: objective.objectiveVideosDetails
							};
						})
					);
				if (this.selectedLesson) {
					this.isSelectedLessonData = true;
				}
			},
			getObjectiveVideo(objective) {
				const clickedObjective = objective;
				this.clickedObjectiveId = clickedObjective.id;
				objective.objectiveDone = this.objectiveDone;
				this.selectedObjectiveURL = objective.objectiveVideosDetails.map(el => el.url);
			},
			done(selectedObjectiveId) {
				const copy = this.selectedLesson.map(item =>
					item.map(el => {
						if (el.id === selectedObjectiveId) {
							this.objectiveDone = true;
						}
						return this.objectiveDone;
					})
				);
				console.log(JSON.parse(JSON.stringify(copy)));
			},
			notDoing() {
				this.objectiveNotDoing = !this.objectiveNotDoing;
				console.log(this.objectiveNotDoing);
				return this.objectiveNotDoing;
			},
			nextClass() {
				this.objectiveNextClass = !this.objectiveNextClass;
				console.log(this.objectiveNextClass);
				return this.objectiveNextClass;
			}
		},
		watch: {
			details() {
				this.details.forEach(item => {
					item.objectiveDetails.forEach(el =>
						el.objectiveVideosDetails.forEach(objective => {
							/* ---------------------------------------------------------------------
	           Vimeo link returned by the API doesn't allow to display it in a url.
	           Removing the string after the last backslash in the URL and replacing
	           'vimeo.com' with 'player.vimeo.com/video' to correctly embed the videos.
	           ------------------------------------------------------------------------ */
							const regex = /vimeo[.]com/gi;
							objective.url = objective.url
								.substring(0, objective.url.lastIndexOf("/") + 1)
								.replace(regex, "player.vimeo.com/video");
						})
					);
				});
				this.lessonsList = this.details;
			}
		},

		props: ["details"]
	};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
	.check-icon,
	.cross-icon,
	.edit-icon {
		background-position: center;
		transition: background 0.9s;
		position: absolute;
		bottom: -10px;
		cursor: pointer;
	}
	.check-icon,
	.cross-icon {
		border-radius: 50%;
	}
	.check-icon:hover {
		background: #81e26e radial-gradient(circle transparent 1%, #81e26e 1%) center/15000%;
	}
	.check-icon:active {
		background-color: #81e26e;
		transition: background 0.2s;
	}

	.cross-icon:hover {
		background: #ff5a2d radial-gradient(circle transparent 1%, #ff5a2d 1%) center/15000%;
	}
	.cross-icon:active {
		background-color: #ff5a2d;
		transition: background 0.2s;
	}

	.edit-icon:hover {
		background: #e2e2e2 radial-gradient(circle transparent 1%, #e2e2e2 1%) center/15000%;
	}
	.edit-icon:active {
		background-color: #e2e2e2;
		transition: background 0.2s;
	}

	.cross-icon {
		color: #f05228;
		left: 85px;
		background: linear-gradient(white, white) center / 50% 80% no-repeat;
	}
	.check-icon {
		color: #6ac259;
		background: linear-gradient(white, white) center / 50% 80% no-repeat;
	}
	.edit-icon {
		color: #017296;
		left: 150px;
	}
	.doneBorder {
		border: 3px solid #81e26e;
	}
	.notDoingBorder {
		border: 3px solid #f05228;
	}
	.nextClassBorder {
		border: 3px solid #017296;
	}
	.objective-body {
		display: flex;
		margin-left: -6.5em;
		margin-top: 1em;
	}
	.menu-container {
		background: #ffd05b;
		width: 200px;
		padding: 2em;
	}
	.objective-container {
		cursor: pointer;
		position: relative;
		height: fit-content;
		display: flex;
		flex-direction: column;
		width: 150px;
		padding: 1em;
		background: #fff;
		box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
		margin-top: 4em;
	}
	.objective-title {
		font-weight: bold;
		font-size: 1em;
	}
	.objective-duration {
		font-weight: bold;
		font-size: 1em;
		margin-left: 1.5em;
	}
	.details-container {
		margin-left: 7em;
	}
	.lesson-container {
		display: inline;
	}
	.lessons {
		display: inline-block;
		font-weight: bold;
		font-size: 1.5em;
	}
	.lesson-number-container {
		margin-left: 1em;
		align-items: center;
		display: inline-flex;
		width: 400px;
		height: 100px;
	}
	.activeLessonNumber {
		background: white;
	}
	.activeLessonNumber,
	.lesson-numbers {
		cursor: pointer;
		font-weight: bold;
		box-shadow: 0px 4px rgba(0, 0, 0, 0.2), 1px 3px rgba(0, 0, 0, 0.1);
		border-radius: 50%;
		margin-left: 0.5em;
		height: 42px;
		font-size: 1.7em;
		width: 50px;
	}
	.lesson-numbers {
		background: #ff9811;
	}
	.video-container {
		display: flex;
		align-items: flex-end;
		flex-grow: 1;
		border: 1px solid #ccc;
	}
	.video-list {
		width: 50%;
		height: 15%;
		margin: 2em 2em 1em 2em;
	}
	.video-background {
		position: relative;
		z-index: 2;
		bottom: 10%;
		left: -6%;
	}
	.exit-session-btn {
		box-shadow: 5px 5px 5px 0px rgba(0, 0, 0, 0.2), -1px -11px 20px 0px rgba(0, 0, 0, 0.1);
		padding: 0.7em 1em;
		color: white;
		margin-left: 3.2em;
		font-weight: bold;
		background: #c92b2b;
		font-size: 1em;
		cursor: pointer;
		outline: 0;
	}
</style>
