<template>
  <div class="customers container">
    <Alert v-if="alert" :message="alert" />
    <h1 class="page-header">Manage customers</h1>
    <input class="form-control" placeholder="enter last name" v-model="filterInput" />
    <br />
    <table class="table table-striped">
      <thead>
        <tr>
          <th>First name</th>
          <th>Last name</th>
          <th>Email</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="customer in filterBy(customers, filterInput)" :key="customer.id">
          <td>{{customer.first_name}}</td>
          <td>{{customer.last_name}}</td>
          <td>{{customer.email}}</td>
          <td>
            <router-link class="btn btn-primary" :to="'/customer/' + customer.id">View</router-link>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Alert from "./Alert";
export default {
  name: "customers",
  data() {
    return {
      customers: [],
      alert: "",
      filterInput: ""
    };
  },
  methods: {
    fetchCustomers() {
      this.$http
        .get("http://localhost/slim/slimapp/public/index.php/api/customers")
        .then(function(response) {
          this.customers = response.body;
        });
    },
    filterBy(list, value) {
      if (value) {
        value = value.charAt(0).toUpperCase() + value.slice(1);
      }
      return list.filter(function(customer) {
        return customer.last_name.indexOf(value) > -1;
      });
    }
  },
  created: function() {
    if (this.$route.query.alert) {
      this.alert = this.$route.query.alert;
    }
    this.fetchCustomers();
  },
  updated: function() {
    this.fetchCustomers();
  },
  components: {
    Alert
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
