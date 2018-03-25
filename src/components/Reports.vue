<template lang="pug">
  #reports
    h4 Reports for order no.&nbsp;
      strong {{ order.orderNumber }}
    br
    .card(v-if='reports.length > 0', v-for='report in reports', :key='report.uuid')
      .card-header {{ report.display }}
      .card-body
        p 
          strong Status:&nbsp;
          | {{ report.status }}

        p.card-text(v-html='report.body')
      .card-footer
        button.btn.btn-info(
          type='button',
          v-on:click="generatePDF(report)"
        ) Generate PDF

    .alert.alert-info(v-if='reports.length == 0') No reports found

    PdfTemplate(v-if="pdf_template", :report="report", :order="order")
</template>

<script>
import PdfTemplate from "./PdfTemplate.vue";
import jsPDF from "jspdf";

export default {
  name: "Reports",
  props: {
    order: {
      type: Object,
      default: {}
    }
  },
  watch: {
    order: function() {
      this.getReports();
    }
  },
  data() {
    return {
      API_URL: process.env.VUE_APP_API_URL,
      reports: [],
      report: {},
      pdf_template: false
    };
  },
  methods: {
    getReports: function() {
      this.$http
        .get(`${this.API_URL}/radiologyreport?v=full&totalCount=true`)
        .then(function(data) {
          this.reports = data.body.results.filter(
            i => i.radiologyOrder.uuid == this.order.uuid
          );
          setTimeout(() => window.scrollTo(0, document.body.scrollHeight), 100);
        });
    },
    generatePDF: function(report) {
      let pdf = new jsPDF();
      this.report = report
      this.pdf_template = true;
      // pdf.save(this.order.orderNumber + '.pdf')
    }
  },
  created: function() {
    this.getReports();
  },
  components: {
    PdfTemplate
  }
};
</script>

<style lang="sass" scoped>
.card
  margin-bottom: 1em
</style>
