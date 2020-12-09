<template>
  <div id="container">
    <topBar />
    <nuxt />
  </div>
</template>

<script>
import topBar from "@/layouts/partials/topBar.vue";
export default {
  components: {
    topBar
  },
  data() {
    return {
      nav: [
        {
          title: "Home",
          icon: "home",
          to: { name: "index" }
        }
      ]
    };
  }
};
</script>
