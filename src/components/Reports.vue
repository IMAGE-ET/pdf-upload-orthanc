<template lang="pug">
  #reports
    h4 Reports for order no.&nbsp;
      strong {{ orderNumber }}
    br
    .card(v-if='reports.length > 0', v-for='report in reports', :key='report.uuid')
      .card-header {{ report.display }}
      .card-body
        p 
          strong Status:&nbsp;
          | {{ report.status }}

        p.card-text(v-html='report.body')
      .card-footer
        button.btn.btn-info(type='button') Generate PDF

    .alert.alert-info(v-if='reports.length == 0') No reports found
</template>

<script>
export default {
  name: "Reports",
  props: {
    orderNumber: {
      type: String,
      default: ""
    }
  },
  watch: {
    orderNumber: function() {
      this.getReports();
    }
  },
  data() {
    return {
      API_URL: process.env.VUE_APP_API_URL,
      reports: []
    };
  },
  methods: {
    getReports: function() {
      this.$http
        .get(`${this.API_URL}/radiologyreport?v=full&totalCount=true`)
        .then(function(data) {
          this.reports = data.body.results.filter(
            i => i.display.split(",")[0] == this.orderNumber
          );
          setTimeout(() => window.scrollTo(0, document.body.scrollHeight), 100);
        });
    }
  },
  created: function() {
    this.getReports();
  }
};
</script>

<style lang="sass" scoped>
.card
  margin-bottom: 1em
</style>
