<template lang="en">
    <div>
      <div class="row ml-1">
        <router-link to="/supplier/add" class="btn btn-primary">Add Supplier</router-link>
      </div>
      <input type="text" v-model="searchTerm" class="form-control mt-2" placeholder="Search Here.." style="width:300px;" />
          <div class="row mt-3">
            <div class="col-lg-12 mb-4">
              <!-- Simple Tables -->
              <div class="card">
                <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
                  <h6 class="m-0 font-weight-bold text-primary">Suppliers List</h6>
                </div>
                <div class="table-responsive">
                  <table class="table align-items-center table-flush">
                    <thead class="thead-light">
                      <tr>
                        <th>Name</th>
                        <th>Photo</th>
                        <th>Phone</th>
                        <th>Address</th>
                        <th>Shop Name</th>
                        <th class="text-center">Action</th>
                      </tr>
                    </thead>
                    <tbody>
                    
                      <tr v-for="supplier in filterSearch" :key="supplier.id">
                        <td>{{ supplier.name }}</td>
                        <td><img :src="supplier.photo" v-if="supplier.photo" id="emp_photo"></td>
                        <td>{{ supplier.phone }}</td>
                        <td>{{ supplier.address }}</td>
                        <td>{{ supplier.shopname }}</td>
                        <td class="text-center">
                          <router-link :to="{ name: 'edit-supplier', params:{id: supplier.id}}" class="btn btn-sm btn-primary">Edit</router-link>
                          <a href="#" @click="deleteSupplier(supplier.id)" class="btn btn-sm btn-danger">Delete</a>
                        </td>
                      </tr>  
                      
                    </tbody>
                  </table>
                </div>
                <div class="card-footer"></div>
              </div>
            </div>
          </div>
          <!--Row-->

    </div>
</template>

<script>
export default {
  created(){
    if(!User.loggedIn()){
      this.$router.push({ name: '/' })
    }
  },
  data() {
    return {
      suppliers: [],
      searchTerm: '',
      response: null,
    }
  },  
  computed: {
    filterSearch() {
      return this.suppliers.filter(supplier => {
        return supplier.name.match(this.searchTerm)
      })
    }
  },
  methods: {
    getAllSuppliers() {
      axios.get('/api/supplier')
      .then(({data}) => (this.suppliers = data))
      .catch()
    },
    deleteSupplier(id) {
          Swal.fire({
            title: 'Are you sure?',
            text: "You won't be able to revert this!",
            icon: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#3085d6',
            cancelButtonColor: '#d33',
            confirmButtonText: 'Yes, delete it!'
          }).then((result) => {
            if (result.isConfirmed) {
              axios.delete('/api/supplier/'+id )
              .then(() => {
                this.suppliers = this.suppliers.filter(supplier => {
                  return supplier.id != id;
                })
              })
              .catch(() => {
                this.$router.push({name: 'suppliers'})
              })
              Swal.fire(
                'Deleted!',
                'Supplier data has been deleted.',
                'success'
              )
            } 
          })
    }
  },
  // Update employees after adding new employee
  created() {
    this.getAllSuppliers();
  },

}
</script>

<style type="text/css">
    #emp_photo {
    height: 60px;
    width: 60px;
    border: 1px solid black;
    box-shadow: 1px 1px cyan;
    border-radius: 6px 5px;
    }
</style>