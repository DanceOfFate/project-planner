<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete" />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from "@/components/SingleProject.vue"
import FilterNav from "@/components/FilterNav.vue";
export default {
  name: 'HomeView',
  data() {
    return {
      projects: [],
      current: 'all'
    }
  },
  components: {SingleProject, FilterNav},
  mounted() {
    fetch('http://localhost:3000/projects')
        .then(response => response.json())
        .then(data => this.projects = data)
        .catch(err => console.log(err.message))
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project) => {
        return project.id !== id;
      })
    },
    handleComplete(id) {
      let p = this.projects.find(project => {
        return project.id === id;
      })
      p.completed = !p.completed;
    }
  },
  computed: {
    filteredProjects() {
      if (this.current === 'completed') {
        return this.projects.filter(project => project.completed)
      }
      if (this.current === 'ongoing') {
        return this.projects.filter(project => !project.completed)
      }

      return this.projects
    }
  }
}
</script>
