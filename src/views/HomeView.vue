<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects.length">
      <div v-for="project in showFilteredProjects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="handleDelete"
          @complete="handleComplete"
        />
      </div>
      <button class="button" @click="addProject">+ Add Projects</button>
    </div>
  </div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue";
import FilterNav from "@/components/FilterNav.vue";
// @ is an alias to /src

export default {
  name: "HomeView",
  components: { SingleProject, FilterNav },
  data() {
    return {
      projects: [],
      current: "all",
    };
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err));
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

      // console.log(p)
    },
    addProject() {
      this.$router.push("/add-project");
    },
  },
  computed: {
    showFilteredProjects() {
      if (this.current === "completed") {
        return this.projects.filter((project) => project.complete);
      } else if (this.current === "ongoing"){
        return this.projects.filter((project) => !project.complete);
      }return this.projects
    },
  },
};
</script>

<style>
.button {
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  font-size: 1.2em;
  margin: auto;
  padding: 10px;
  background: #76e1a4;
  color: aliceblue;
  border-style: none;
  border-radius: 3px;
  max-width: 170px;
  cursor: pointer;
}
</style>
