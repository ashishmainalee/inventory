<template lang="en">
    <div>
      <div class="row justify-content-center">
      <div class="col-xl-6 col-lg-12 col-md-9">
        <div class="card shadow-sm my-5">
          <div class="card-body p-0">
            <div class="row">
              <div class="col-lg-12">
                <div class="login-form">
                  <div class="text-center">
                    <h1 class="h4 text-gray-900 mb-4">Login</h1>
                  </div>
                  <form class="user" @submit.prevent="handleLogin">
                    <div class="form-group">
                      <input type="email" class="form-control" id="exampleInputEmail" aria-describedby="emailHelp"
                        placeholder="Enter Email Address" v-model="form.email">
                    <small class="text-danger" v-if="errors.email">{{ errors.email[0] }}</small>
                    </div>
                    <div class="form-group">
                      <input type="password" class="form-control" id="exampleInputPassword" placeholder="Password" v-model="form.password">
                      <small class="text-danger" v-if="errors.password">{{ errors.password[0] }}</small>
                    
                    </div>
                    <div class="form-group">
                      <div class="custom-control custom-checkbox small" style="line-height: 1.5rem;">
                        <input type="checkbox" class="custom-control-input" id="customCheck">
                        <label class="custom-control-label" for="customCheck">Remember
                          Me</label>
                      </div>
                    </div>
                    <div class="form-group">
                      <button type="submit" value="submit" class="btn btn-primary btn-block">Login</button>
                    </div>
                    <hr>
                  </form>
                  <div class="text">
                    <router-link to="/register" class="font-weight-bold small">Create an Account!</router-link>
                  </div>
                  <div class="text">
                    <router-link to="/forgotpassword" class="font-weight-bold small">Forgot Password?</router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    </div>
</template>

<script type="text/javascript">
export default {
  created(){
    if(User.loggedIn()){
      this.$router.push({ name: 'home' })
    }
  },
  data(){
    return {
      form: {
        email: null,
        password: null
      },
      errors: {

      }
    }
  },
  methods: {
    handleLogin() {
      axios.post('/api/auth/login', this.form)
      .then(response => {
        User.responseAfterLogin(response)
        Toast.fire({
          icon: 'success',
          title: 'Signed in successfully'
        })
        this.$router.push({ name: 'home' })
      })
      .catch(error => this.errors = error.response.data.errors)
      .catch(
          Toast.fire({
          icon: 'warning',
          title: 'Invalid email or password'
        })
      )
    }
  },
}
</script>

<style lang="en">
    
</style>