<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <Project :project="project" @delete="handleDelete" @complete="handleComplete" />
      </div>
    </div>
    <div v-else>Loading projects</div>
  </div>
</template>

<script>
import Project from "../components/Project.vue";
export default {
  name: "Home",
  components: { Project },
  data() {
    return {
      projects: [],
    };
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project) => {
        return project.id !== id;
      });
    },
    handleComplete(id) {
      let p = this.projects.find((project) => {
        return project.id === id;
      });
      p.complete = !p.complete;
    },
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch((error) => console.log(error.message));
  },
};
</script>

<style lang="scss" scoped>
</style>
