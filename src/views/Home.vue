<template>
  <div class="home">
    <img
      alt="Vue logo"
      src="../assets/logo.png"
    />
  </div>
</template>

<script>
// import TournamentListElement from '@/components/TournamentListElement.vue';

export default {
  name: 'Home',
  components: {
    // TournamentListElement
  },
  data() {
    return {};
  },

  created() {
    this.init();
  },

  methods: {
    init() {}
  }
};
</script>
