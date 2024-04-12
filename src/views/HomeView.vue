<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <SingleProject :project="project" />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from "@/components/SingleProject.vue"
export default {
  name: 'HomeView',
  data() {
    return {
      projects: []
    }
  },
  components: {SingleProject},
  mounted() {
    fetch('http://localhost:3000/projects')
        .then(response => response.json())
        .then(data => this.projects = data)
        .catch(err => console.log(err.message))
  }
}
</script>
