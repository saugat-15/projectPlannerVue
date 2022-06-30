<template>
  <div
    class="project"
    v-show="projectDetails"
    :class="{ complete: project.complete }"
  >
    <div class="actions">
      <h3 @click="toggleDetails">{{ project.title }}</h3>
      <div class="material-icons">
        <router-link :to="{name: 'EditProject', params: {id: project.id}}">
          <span @click="editProject" class="material-icons"> edit </span>
        </router-link>

        <span @click="deleteProject" class="material-icons"> delete </span>
        <span @click="toggleComplete" class="material-icons tick"> done </span>
      </div>
    </div>
    <div class="details" v-if="showDetails">
      <p>{{ project.details }}</p>
      <p></p>
    </div>
  </div>
</template>

<script>
export default {
    props: ["project"],
    data() {
        return {
            showDetails: false,
            projectDetails: true,
            uri: "http://localhost:3000/projects/" + this.project.id,
        };
    },
    methods: {
        toggleDetails() {
            this.showDetails = !this.showDetails;
        },
        deleteProject() {
            fetch(this.uri, { method: "DELETE" })
                .then(() => this.$emit("delete", this.project.id))
                .catch((err) => console.log(err.message));
        },
        toggleComplete() {
            fetch(this.uri, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ complete: !this.project.complete })
      }).then(() => {
        this.$emit('complete', this.project.id)
      }).catch(err => console.log(err))
      },
      editProject() {  
        }
    }
}
</script>

<style>
.project {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
  /* cursor: pointer; */
}
/* completed projects */
.project.complete {
  border-left: 4px solid #03c07e;
  /* cursor: pointer; */
}

.project.complete .tick {
  color: #14e89e;
  /* cursor: pointer; */
}

h3 {
  cursor: pointer;
}

.actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.material-icons {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}
.material-icons:hover {
  color: #777;
}
</style>
