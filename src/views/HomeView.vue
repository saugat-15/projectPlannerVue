<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
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
// @ is an alias to /src

export default {
  name: "HomeView",
  components: { SingleProject },
  data() {
    return {
      projects: [],
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
      this.$router.push('/add-project')
    }
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
