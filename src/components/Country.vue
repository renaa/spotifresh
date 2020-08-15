<template>
  <div class="country" @click="emitCounry">
    {{ id }}
  </div>
</template>

<script>
export default {
  props: {
    id: String,
  },

  methods: {
    emitCounry() {
      this.$emit("emit-country", this.id)
    },
  },
}
</script>

<style scoped lang="scss">
.country {
  color: white;
  padding: 0.15em;
  text-align: center;
  height: 100%;
  cursor: pointer;
}
.country:hover {
  color: #22ff00;
  background-color: black;
}
</style>
