<style>
   .widget-user-header{
   background-position: center center;
   background-size: cover;
   height: -webkit-fill-available;
   }
</style>
<template>
   <div class="container">
      <div class="row justify-content-center">
         <div class="col-md-12 mt-3">
            <div class="card card-widget widget-user">
               <!-- Add the bg color to the header using any of the bg-* classes -->
               <div class="widget-user-header text-white" style="background-image:url('/img/macbook-pro.jpg')">
                  <h3 class="widget-user-username">Elizabeth Pierce</h3>
                  <h5 class="widget-user-desc">Web Designer</h5>
               </div>
               <div class="widget-user-image">
                  <img class="img-circle" :src="getProfilePhoto()" alt="User Avatar">
               </div>
               <div class="card-footer">
                  <div class="row">
                     <div class="col-sm-4 border-right">
                        <div class="description-block">
                           <h5 class="description-header">3,200</h5>
                           <span class="description-text">SALES</span>
                        </div>
                        <!-- /.description-block -->
                     </div>
                     <!-- /.col -->
                     <div class="col-sm-4 border-right">
                        <div class="description-block">
                           <h5 class="description-header">13,000</h5>
                           <span class="description-text">FOLLOWERS</span>
                        </div>
                        <!-- /.description-block -->
                     </div>
                     <!-- /.col -->
                     <div class="col-sm-4">
                        <div class="description-block">
                           <h5 class="description-header">35</h5>
                           <span class="description-text">PRODUCTS</span>
                        </div>
                        <!-- /.description-block -->
                     </div>
                     <!-- /.col -->
                  </div>
                  <!-- /.row -->
               </div>
            </div>
         </div>
         <div class="col-md-12 mt-3">
            <div class="card">
               <div class="card-header p-2">
                  <ul class="nav nav-pills">
                     <li class="nav-item"><a class="nav-link" href="#activity" data-toggle="tab">Activity</a></li>
                     <li class="nav-item"><a class="nav-link active show" href="#settings" data-toggle="tab">Settings</a></li>
                  </ul>
               </div>
               <!-- /.card-header -->
               <div class="card-body">
                  <div class="tab-content">
                     <div class="tab-pane active show" id="settings">
                        <form class="form-horizontal">
                           <div class="form-group">
                              <label for="inputName" class="col-sm-2 control-label">Name</label>
                              <div class="col-sm-10">
                                 <input type="text" v-model="form.name" :class="{ 'is-invalid': form.errors.has('name') }" class="form-control" id="inputName" placeholder="Name">
                                 <has-error :form="form" field="name"></has-error>
                              </div>
                           </div>
                           <div class="form-group">
                              <label for="inputEmail" class="col-sm-2 control-label">Email</label>
                              <div class="col-sm-10">
                                 <input type="email" v-model="form.email" class="form-control" :class="{ 'is-invalid': form.errors.has('email') }" id="inputEmail" placeholder="Email">
                                <has-error :form="form" field="email"></has-error>
                              </div>
                           </div>
                           <div class="form-group">
                              <label for="inputExperience" class="col-sm-2 control-label" >Experience</label>
                              <div class="col-sm-10">
                                 <textarea class="form-control" v-model="form.bio" :class="{ 'is-invalid': form.errors.has('bio') }" id="inputExperience" placeholder="Experience"></textarea>
                                <has-error :form="form" field="bio"></has-error>
                              </div>
                           </div>

                            <!-- <div class="form-group">
                                <div class="col-sm-10">
                                    <label for="exampleInputFile">Profile Photo</label>
                                    <div class="input-group">
                                        <div class="custom-file">
                                            <input type="file" class="custom-file-input" id="exampleInputFile">
                                            <label class="custom-file-label" for="exampleInputFile">Choose file</label>
                                        </div>
                                        <div class="input-group-append">
                                            <span class="input-group-text" id="">Upload</span>
                                        </div>
                                    </div>
                                </div>
                            </div> -->

                            <div class="form-group">
                                <div class="col-sm-10">
                                    <div class="card border-light">
                                        <div class="card-body">
                                            <label for="photo" class="col-sm-2">Profile Photo</label>
                                            <input type="file" @change="updateProfilePhoto" id="photo">
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <div class="form-group">
                                <label for="password" class="col-sm-10 control-label">Password(leave empty if not changing</label>
                                <div class="col-sm-10">
                                    <input type="password" v-model="form.password" :class="{ 'is-invalid': form.errors.has('email') }" class="form-control" id="password" placeholder="password">
                                    <has-error :form="form" field="password"></has-error>
                                </div>
                            </div>

                           <div class="form-group">
                              <div class="col-sm-offset-2 col-sm-10">
                                 <div class="checkbox">
                                    <label>
                                    <input type="checkbox"> I agree to the <a href="#">terms and conditions</a>
                                    </label>
                                 </div>
                              </div>
                           </div>
                           <div class="form-group">
                              <div class="col-sm-offset-2 col-sm-10">
                                 <button type="submit" @click.prevent="updateInfo" class="btn btn-success">Update</button>
                              </div>
                           </div>
                        </form>
                     </div>
                    <div class="tab-pane active show" id="activity">
                            My Activity
                     </div>
                     <!-- /.tab-pane -->
                  </div>
                  <!-- /.tab-content -->
               </div>
               <!-- /.card-body -->
            </div>
            <!-- /.nav-tabs-custom -->
         </div>
      </div>
   </div>
</template>
<script>
   export default {

       data(){
           return{
                form: new Form({
                    id:'',
                    name: '',
                    email: '',
                    password: '',
                    type: '',
                    bio: '',
                    photo: ''
                })
           }
       },

       mounted() {
           console.log('Component mounted.')
       },

       created(){
           axios.get("api/profile").then(({ data }) => (this.form.fill(data)));
       },

       methods:{

           getProfilePhoto(){

               let photo = (this.form.photo.length) > 100 ? this.form.photo : "img/profile/" + this.form.photo;
            //    return "img/profile/" + this.form.photo;
                return photo;
           },

           updateInfo(){
               this.$Progress.start();
               this.form.put('api/profile')
               .then(() => {
                   Fire.$emit('fireLoadUsers');
                   this.$Progress.finish();
               })
               .catch(() => {
                    this.$Progress.fail();
               })
           },

           updateProfilePhoto(e){
                //    console.log('uploading');
                let file = e.target.files[0];
                // console.log(file);
                let reader = new FileReader();

                   if(file['size'] < 2111775){
                        reader.onloadend = (file) => {
                            // console.log('RESULT', reader.result)
                            this.form.photo = reader.result;
                        }
                        reader.readAsDataURL(file);
                    }else{
                        swal({
                        title: 'Ooppss',
                        text: "You are uploading a large file!",
                        type: 'error',

                        });
                }

           }
       },
   }


</script>



