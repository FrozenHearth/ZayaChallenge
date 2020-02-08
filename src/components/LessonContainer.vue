<template>
  <div class="lesson-container">
    <div class="container">
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
      lessonDetailsData: []
    };
  },
  // props: {},
  mounted() {
    axios.get("https://api.myjson.com/bins/qubzl").then(res => {
      this.lessonDetailsData = res.data.lessonDetails;
      this.recitalTitle = res.data.recitalTitle;
      this.instrumentTitle = res.data.instrumentTitle;
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
.recital-title {
  margin-left: auto;
  margin-right: auto;
}
.container {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
