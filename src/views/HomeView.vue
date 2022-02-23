<template>
  <div class="home">
    <Ranking />
  </div>
</template>

<script>
// @ is an alias to /src
import Ranking from "@/components/Ranking.vue";

export default {
  name: "HomeView",
  components: {
    Ranking,
  },
};
</script>
