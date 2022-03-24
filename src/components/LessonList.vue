<style>
  @import '../assets/css/styles.css';
</style>

<template> 
  <main>
    <div id="main" v-for="(lesson, index) in lessons" :key="index">
      <h1>Subject Name: {{lesson.Subject}}</h1>
      <h2>Location: {{lesson.Location}}</h2>
      <h3>Price: £{{lesson.Price}}</h3>
      <h4 id="Space">Space: {{lesson.Space}}</h4>
      <figure>
        <img v-bind:src="lesson.Image" height="100" width="100" />
      </figure>
      <button id="add" @click="addLesson(lesson), reduce(index)">Add to cart</button>
    </div>
  </main>
</template>

<script>
export default {
  name: "LessonList",
  props: {
  },
  data() {
    return {
      lessons: [],
    }
  },
  created: function () {
            var vm = this;
            fetch('https://rabbiaapp2.herokuapp.com/collection/Lessons').then(
              function (response) {   //Returns the result from the Server when the fetch promise resolves.
                response.json().then(
                  function (json) {
                    console.log(`${vm.lessons}`)
                                  //Saving the return JSON Object from the Server to the lessons vue data member.
                  vm.lessons = json;   //Lesson app is the name of the VUE Object and lessons is its data member.
                });
              })   
  },
  methods: {
    addLesson(lesson) {
        this.$emit('addLesson', lesson);
    },
    reduce(index) {
      this.lessons[index].Space--;
      if (this.lessons[index].Space == 0) {
        document.getElementById("Space").innerHTML = "<h4>Spaces Full</h4>"
        document.getElementById("add").style.visibility = "hidden";
      } 
    }
  },
};

</script>
