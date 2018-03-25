<template lang="pug">
  #orders
    table.table.table-hover
      thead.thead-dark
        tr
          th(scope='col', nowrap) Order #
          th(scope='col') Concept
          th(scope='col') Patient
          th(scope='col') Reason
          th(scope='col') Urgency
          th(scope='col') Activated on
          th(scope='col')
      tbody
        tr(scope='row', v-for='order in orders', :key='order.orderNumber')
          td(nowrap) {{ order.orderNumber }}
          td {{ order.concept.display }}
          td(nowrap) {{ order.patient.display | name }}
          td {{ order.orderReasonNonCoded }}
          td {{ order.urgency }}
          td(nowrap) {{ order.dateActivated | date }}
          td
            button.btn.btn-primary(
              type="button",
              v-on:click="getReports(order.orderNumber)"
            ) View reports
    #reports-container
      Reports(v-if="currentOrderNumber", :orderNumber="currentOrderNumber")
</template>

<script>
import Reports from "./Reports.vue";

export default {
  name: "Orders",
  data() {
    return {
      API_URL: process.env.VUE_APP_API_URL,
      orders: [],
      currentOrderNumber: ""
    };
  },
  methods: {
    getOrders: function() {
      this.$http
        .get(`${this.API_URL}/radiologyorder?totalCount=true&v=full`)
        .then(function(data) {
          this.orders = data.body.results;
        });
    },
    getReports: function(orderNumber) {
      this.currentOrderNumber = orderNumber;
    }
  },
  created: function() {
    this.getOrders();
  },
  filters: {
    name: value => value.split("-")[1],
    date: value => new Date(value).toLocaleString()
  },
  components: {
    Reports
  }
};
</script>

<style lang="sass" scoped>
  table
    text-align: center

  #reports-container
    padding: 1em
</style>
