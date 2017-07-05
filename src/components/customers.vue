<template>
  <div class="customers container">
    <Alert v-if="alert" v-bind:message="alert" />
   	<h1 class="page-header">Manage Customers</h1>
    <div class="row">
    <div class="col-md-4">
    <input class="form-control" placeholder="Enter First Name" v-model="filterInput">
    </div>
    <div class="col-md-8"></div>
    </div>
    <br />
   	<div class="table-responsive">
   		<table class="table table-striped">
   			<thead>
   				<tr>
   					<th>First Name</th>
   					<th>Last Name</th>
   					<th>Email</th>
   					<th></th>
   				</tr>
   			</thead>
   			<tbody>
   				<tr v-for="customer in filterBy(customers, filterInput)">
   					<td>{{customer.first_name}}</td>
   					<td>{{customer.last_name}}</td>
   					<td>{{customer.email}}</td>
   					<td><router-link class="btn btn-default" v-bind:to="'/customer/'+customer.id">View</router-link></td>
   				</tr>
   			</tbody>
   		</table>
   	</div>
  </div>
</template>

<script>
import Alert from './Alert';
export default {
  name: 'customers',
  data () {
    return {
      // bringing customer from API
      customers: [],
      alert: '',
      filterInput:''
    }
  },
  methods: {
  	fetchCustomers(){
  		this.$http.get('http://slimapp1/api/customers')
  		.then(function(response){
        this.customers = response.body;
  		});
  	},
    filterBy(list, value){
      value = value.charAt(0).toUpperCase(1);
      return list.filter(function(customer){
        return customer.first_name.indexOf(value) > -1;
      });
    }
  },
  created: function(){
    this.fetchCustomers();
    if (this.$route.query.alert) {
      this.alert = this.$route.query.alert;
     }
  	this.fetchCustomers();
  },
  updated: function(){
    this.fetchCustomers();
  },
  components: {
    Alert
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
