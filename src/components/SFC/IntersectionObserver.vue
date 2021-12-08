<template>
  <div></div>
</template>

<script>
export default {
  name: "IntersectionObserver",

  props: {
    active: {
      type: Boolean,
    },
  },

  data() {
    return {
      observer: null,
    };
  },

  methods: {
    onIntersection(entries) {
      if (this.active && entries[0].isIntersecting) {
        this.$emit("intersecting");
      }
    },
  },

  mounted() {
    this.observer = new IntersectionObserver(this.onIntersection);
    this.observer.observe(this.$el);
  },
};
</script>

<style scoped>
div {
  width: 0;
  height: 0;
}
</style>
