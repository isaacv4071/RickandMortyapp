<template>
  <div class="column is-12-mobile is-4-desktop is-4-tablet">
    <div class="card">
      <div class="card-header">
        <img v-bind:src="character.image" v-bind:alt="character.name"/>
      </div>
      <div class="card-content">
        <h3 class="title is-size-4">{{ character.name }}</h3>
        <button class="button is-success is-rounded is-small" @click="showMore(character.id)">ver mas</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
  img{
    width: 100%;
    padding: 10px;
  }
</style>

<script>
export default {
  props: ["character"],
  methods: {
    showMore(id) {
      this.$emit("showModal", id);
    }
  }
};
</script>
