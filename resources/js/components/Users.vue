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
                      <th>Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td>183</td>
                      <td>John Doe</td>
                      <td>11-7-2014</td>
                      <td><span class="tag tag-success">Approved</span></td>
                      <td>
                          <a href="#">
                              <i class="fa fa-edit blue"></i>
                          </a>
                          |
                          <a href="#">
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
                    <div class="modal-body">
                      <div class="form-group">
                        <input v-model="form.name" type="text" name="name" placeholder="Enter Name" class="form-control">
                        <div v-if="form.errors.has('name')" v-html="form.errors.get('name')" />
                      </div>
                      <div class="form-group">
                        <input v-model="form.name" type="email" name="email" placeholder="Enter Email" class="form-control">
                        <div v-if="form.errors.has('email')" v-html="form.errors.get('email')" />
                      </div>
                      <div class="form-group">
                        <input v-model="form.name" type="password" name="password" placeholder="Enter Password" class="form-control">
                        <div v-if="form.errors.has('password')" v-html="form.errors.get('password')" />
                      </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-danger" data-dismiss="modal">Close</button>
                        <button type="button" class="btn btn-primary">Create</button>
                    </div>
                </div>
            </div>
        </div>
    </div>



</template>

<script>
    export default {
      data: () => ({
        form: new Form({
          name: '',
          email: '',
          password: ''
        })
      }),
      mounted() {
          console.log('Component mounted.')
      }
  }
</script>
