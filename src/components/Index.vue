<template lang="pug">
  #index
    .jumbotron.jumbotron-fluid
      .lead Upload Radiology Report PDF to Orthanc
    Orders
</template>

<script>
import Orders from "./Orders.vue";

export default {
  name: "Index",
  data() {
    return {};
  },
  components: {
    Orders
  }
};
</script>

<style lang="sass" scoped>

  .jumbotron
    padding: 2rem

    .lead
      text-align: center
      font-weight: bold

</style>
