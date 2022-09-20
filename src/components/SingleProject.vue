<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="toggleDetails">
        {{ project.title }}
      </h3>
      <div class="icons">
        <span @click="toggleComplete" class="material-icons tick">
          done
        </span>
        <RouterLink :to="{ name: 'EditProject', params: { id: project.id } }">
          <span class="material-icons">
            edit
          </span>
        </RouterLink>
        <span @click="deleteProject" class="material-icons">
          delete
        </span>
      </div>
    </div>
    <div v-if="isDetailsOpened" class="details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    project: {
      type: Object,
      default: ''
    }
  },
  data () {
    return {
      isDetailsOpened: false,
      uri: 'http://localhost:3000/projects/' + this.project.id
    }
  },
  methods: {
    toggleDetails () {
      this.isDetailsOpened = !this.isDetailsOpened
    },
    deleteProject () {
      fetch(this.uri, { method: 'DELETE' })
        .then(() => this.$emit('delete', this.project.id))
    },
    toggleComplete () {
      fetch(this.uri, {
        method: 'PATCH',
        body: JSON.stringify({ complete: !this.project.complete }),
        headers: { 'Content-type': 'application/json' }
      })
        .then(() => this.$emit('complete', this.project.id))
    }
  }
}
</script>

<style scoped>
.project {
  margin: 20px auto;
  background: #fff;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
}

.project.complete {
  border-left: 4px solid #00ce89;
}

.project.complete .tick {
  color: #00ce89;
}

h3 {
  cursor: pointer;
}

.actions {
  display: flex;
  align-items: center;
  justify-content: space-between;
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