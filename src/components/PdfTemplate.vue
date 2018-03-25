<template lang="pug">
  #pdf-template
    .container
      h2 Radiology report for order no.&nbsp;
        strong {{ order.orderNumber }}
      br
      h5 Order details:

</template>

<script>
export default {
  name: "PdfTemplate",
  props: {
    order: {
      type: Object,
      required: true
    },
    report: {
      type: Object,
      required: true
    }
  }
};
</script>

<style lang="sass" scoped>

</style>

