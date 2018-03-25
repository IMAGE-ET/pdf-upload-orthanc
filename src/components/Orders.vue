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
            button.btn.btn-primary(type="button") View reports
    .container#reports Reports
</template>

<script>
export default {
  name: "Orders",
  data() {
    return {
      API_URL: process.env.VUE_APP_API_URL,
      orders: []
    };
  },
  methods: {
    getOrders: function() {
      this.$http
        .get(`${this.API_URL}/radiologyorder?totalCount=true&v=full`)
        .then(function(data) {
          this.orders = data.body.results;
        });
    }
  },
  created: function() {
    this.getOrders();
  },
  filters: {
    name: function(value) {
      return value.split("-")[1];
    },
    date: function(value) {
      let date = new Date(value);
      return date.toLocaleString();
    }
  }
};
</script>

<style lang="sass" scoped>
  #orders
    text-align: center
</style>
