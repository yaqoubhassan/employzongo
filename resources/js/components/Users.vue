<template>
    <div class="container">
        <div class="row mt-5">
          <div class="col-md-12">
            <div class="card">
              <div class="card-header">
                <h3 class="card-title">Users List</h3>

                <div class="card-tools">
                    <button class="btn btn-success" data-toggle="modal" data-target="#addNew"> Add New <i class="fas fa-user-plus fa-fw"></i></button>
                </div>
              </div>
              <!-- /.card-header -->
              <div class="card-body table-responsive p-0">
                <table class="table table-hover text-nowrap">
                  <thead>
                    <tr>
                      <th>ID</th>
                      <th>Name</th>
                      <th>Email</th>
                      <th>Phone Number</th>
                      <th>Registration Date</th>
                      <th>Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="user in users" :key="user.id">
                      <td>{{user.id}}</td>
                      <td>{{user.name | upText}}</td>
                      <td>{{user.email}}</td>
                      <td><span class="tag tag-success">{{user.phoneNumber ? user.phoneNumber : 'N/A'}} </span></td>
                      <td>{{user.created_at | formatDate}}</td>
                      <td>
                          <a href="#">
                              <i class="fa fa-edit blue"></i>
                          </a>
                          |
                          <a href="#" @click="deleteUser(user.id)">
                              <i class="fa fa-trash red"></i>
                          </a>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
              <!-- /.card-body -->
            </div>
            <!-- /.card -->
          </div>
        </div>
            <!-- Modal -->
        <div class="modal fade" id="addNew" tabindex="-1" aria-labelledby="addNewLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="addNewLabel">Add New User</h5>
                        <!-- <a href="#" class="close" data-dismiss="modal" aria-label="close">&times;</a> -->
                        <!-- <button type="button" class="close" data-dismiss="modal" aria-label="close"></button> -->
                        <button type="button" class="close" aria-label="Close"  data-dismiss="modal">
                          <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <form @submit.prevent="createUser">
                      <div class="modal-body">
                        
                          <div class="form-group">
                            <input v-model="form.name" type="text" name="name" placeholder="Enter Name" class="form-control">
                            <div v-if="form.errors.has('name')" v-html="form.errors.get('name')" style="color:red"/>
                          </div>
                          <div class="form-group">
                            <input v-model="form.email" type="email" name="email" placeholder="Enter Email" class="form-control">
                            <div v-if="form.errors.has('email')" v-html="form.errors.get('email')" style="color:red"/>
                          </div>
                          <div class="form-group">
                            <input v-model="form.phoneNumber" type="phoneNumber" name="phoneNumber" placeholder="Enter Phone Number" class="form-control">
                            <div v-if="form.errors.has('phoneNumber')" v-html="form.errors.get('phoneNumber')" style="color:red"/>
                          </div>
                          <div class="form-group">
                            <input v-model="form.password" type="password" name="password" placeholder="Enter Password" class="form-control">
                            <div v-if="form.errors.has('password')" v-html="form.errors.get('password')" style="color:red"/>
                          </div>
                        
                      </div>
                    
                      <div class="modal-footer">
                          <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                          <button type="submit" class="btn btn-primary">Create</button>
                      </div>
                    </form>
                </div>
            </div>
        </div>
    </div>



</template>

<script>
    export default {
      data: () => ({
        users: {},
        form: new Form({
          name: '',
          email: '',
          phoneNumber: '',
          password: ''
        })
      }),
      methods: {
        deleteUser(id) {
          Swal.fire({
            title: 'Are you sure?',
            text: "You won't be able to revert this!",
            icon: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#3085d6',
            cancelButtonColor: '#d33',
            confirmButtonText: 'Yes, delete it!'
          }).then((result) => {
            if (result.value) {
              this.form.delete('api/users/'+id).then(() => {
              
              Swal.fire(
                'Deleted!',
                'User successfully deleted.',
                'success'
              )
              Fire.$emit('afterCreated');
              
              }).catch(() => {
                swal("Failed!", "There was something wrong.", "warning");
              });
            }            
          })
        },
        loadUsers() {
          axios.get('/api/users').then(({ data }) => (this.users = data.data));
        },
        createUser() {
          this.$Progress.start();
          this.form.post('/api/users')
          .then(() => {
            Fire.$emit('afterCreated');

            $('#addNew').modal('hide')

            Toast.fire({
              icon: 'success',
              title: 'User created successfully'
            });

            this.$Progress.finish();
          })
          .catch(() => {

          })
          
        }
      },
      created() {
          this.loadUsers();
          Fire.$on('afterCreated', () => {
            this.loadUsers();
          });
          // setInterval(() => this.loadUsers(), 3000);
      }
  }
</script>
