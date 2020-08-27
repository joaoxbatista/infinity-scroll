<template>
  <div ref="infinityLoader" class="w-full text-center">
    <img
      class="inline"
      src="@/assets/loader.gif"
      v-if="loading"
      alt="loading icon"
    />
  </div>
</template>

<script>
export default {
  name: "InfinityLoader",
  props: {
    loading: {
      type: Boolean,
      required: true,
      default: false,
    },

    onScroll: {
      type: Function,
      required: true,
      default: () => {},
    },
  },
  mounted() {
    const observer = new IntersectionObserver(([entry]) => {
      if (entry.isIntersecting) {
        this.onScroll();
      }
    });

    const element = this.$refs.infinityLoader;
    observer.observe(element);
  },
};
</script>
