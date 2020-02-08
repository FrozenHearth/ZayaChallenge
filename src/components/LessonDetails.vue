<template>
  <div class="details-container">
    <h2 class="lessons">Lessons</h2>
    <div class="lesson-number-container">
      <div
        v-on:click="getLesson(number)"
        v-bind:key="number.id"
        v-bind:class="
          number.id === clickedLessonId
            ? 'activeLessonNumber'
            : 'lesson-numbers'
        "
        v-for="number in lessonsList"
      >{{ number.lessonTitle.slice(-1) }}</div>
    </div>
    <section class="objective-body" v-if="isData">
      <aside class="menu-container">
        <div class="objective-container" v-bind:key="x.id" v-for="x in selected[0]">
          <span class="objective-title">{{ x.title }}</span>
          <span class="objective-duration">({{ x.duration }} mins)</span>
          <i class="fas fa-check-circle fa-lg check-icon"></i>
          <i class="fas fa-times-circle fa-lg cross-icon"></i>
          <i class="fas fa-edit fa-lg edit-icon"></i>
        </div>
      </aside>
      <main class="video-container"></main>
    </section>
  </div>
</template>

<script>
export default {
  name: "LessonDetails",
  data() {
    return {
      isData: false,
      clickedLessonId: "",
      selected: {},
      lessonsList: []
    };
  },
  methods: {
    getLesson(event) {
      const clickedLesson = JSON.parse(JSON.stringify(event));
      this.clickedLessonId = clickedLesson.id;

      this.selected = this.details
        .filter(x => x.id === clickedLesson.id)
        .map(o =>
          o.objectiveDetails.map(x => ({
            title: x.title,
            duration: x.durationInMinutes,
            id: x.id
          }))
        );
      if (this.selected) {
        this.isData = true;
      }
    }
  },
  watch: {
    details() {
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
  background: #81e26e radial-gradient(circle transparent 1%, #81e26e 1%)
    center/15000%;
}
.check-icon:active {
  background-color: #81e26e;
  transition: background 0.2s;
}

.cross-icon:hover {
  background: #ff5a2d radial-gradient(circle transparent 1%, #ff5a2d 1%)
    center/15000%;
}
.cross-icon:active {
  background-color: #ff5a2d;
  transition: background 0.2s;
}

.edit-icon:hover {
  background: #e2e2e2 radial-gradient(circle transparent 1%, #e2e2e2 1%)
    center/15000%;
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
.objective-body {
  display: flex;
}
.menu-container {
  background: #ffd05b;
  width: 200px;
  padding: 2em;
}
.video-container {
  flex-grow: 1;
  border: 1px solid #ccc;
}
.objective-container {
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
  margin-top: -1em;
}
.lessons {
  display: inline-block;
  font-weight: bold;
  font-size: 1.8em;
}
.lesson-number-container {
  margin-left: 1em;
  align-items: center;
  display: inline-flex;
  width: 500px;
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
  margin-left: 1em;
  height: 42px;
  font-size: 1.7em;
  width: 50px;
}
.lesson-numbers {
  background: #ff9811;
}
</style>
