<template>
  <form @submit.prevent="handleSubmit">
    <label>
      Title:
      <input type="text" v-model="title" required />
    </label>
    <label>
      Details:
      <textarea v-model="details" required></textarea>
    </label>
    <button>Update Project</button>
  </form>
</template>

<script>
export default {
  props: ["id"],
  data() {
    return {
      title: "",
      details: "",
      uri: ` http://localhost:3000/projects/${this.id}`,
    };
  },
  methods: {
    handleSubmit() {
      fetch(this.uri, {
        method: "PATCH",
        headers: { "content-type": "application/json" },
        body: JSON.stringify({ title: this.title, details: this.details }),
      })
        .then(() => this.$router.push("/"))
        .catch((error) => console.log(error));
    },
  },
  mounted() {
    fetch(this.uri)
      .then((res) => res.json())
      .then((data) => {
        this.title = data.title;
        this.details = data.details;
      })
      .catch((error) => console.log(error.message));
  },
};
</script>

<style  scoped>
</style>