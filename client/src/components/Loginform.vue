<template>
 <div class="Login">
  <div class="container">
   <div class="row">
    <div class="col-sm-9 col-md-7 col-lg-5 mx-auto">
     <div class="card card-signin my-5">
      <div class="card-body">
       <h5 class="card-title text-center">LOGIN</h5>
       <form class="form-signin">
        <div class="form-label-group">
         <input type="email" id="loginEmail" class="form-control" placeholder="Email address" v-model="user.email" required autofocus>
         <label for="loginEmail">Email address</label>
        </div>
        <div class="form-label-group">
          <input type="password" id="loginPassword" class="form-control" placeholder="Password" v-model="user.password" required>
          <label for="loginPassword">Password</label>
        </div>
        <button class="btn btn-lg btn-dark btn-block text-uppercase" type="submit" @click.prevent="login">LOGIN</button>
        <div class="link">
         <a href="#" id="link" @click.prevent="dontHaveAcc">Don't Have Account?</a>
        </div>
        <div class="social">
          <GoogleLogin :params="googleSignInParams" :renderParams="renderParams" :onSuccess="onSuccess" :onFailure="onFailure"></GoogleLogin>
        </div>
       </form>
      </div>
     </div>
    </div>
   </div>
  </div>
 </div>
</template>
<script>
import axios from 'axios'
import GoogleLogin from 'vue-google-login'
export default {
  components: {
    GoogleLogin
  },
  name: 'Loginform',
  data () {
    return {
      user: {
        email: '',
        password: ''
      },
      googleSignInParams: {
        client_id: '94289824709-9elnoiuecs7tfdilbrmos1gus49umpr6.apps.googleusercontent.com'
      },
      renderParams: {
        width: 250,
        height: 50,
        longtitle: true
      }
    }
  },
  methods: {
    dontHaveAcc () {
      this.$emit('dontHaveAcc')
    },
    login () {
      axios({
        method : 'POST',
        url: 'https://elixir-fox-kanban.herokuapp.com/login',
        data : {
          email: this.user.email,
          password: this.user.password
        }
      })
      .then((result)=>{
        this.user.email = ''
        this.user.password = ''
        localStorage.setItem('access_token',result.data.access_token)
        this.$emit('logged')
        const Toast = Swal.mixin({
          toast: true,
          position: 'top-end',
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
          onOpen: (toast) => {
            toast.addEventListener('mouseenter', Swal.stopTimer)
            toast.addEventListener('mouseleave', Swal.resumeTimer)
          }
        })
        Toast.fire({
          icon: 'success',
          title: 'Logged in successfully'
        })
      })
      .catch((err)=>{
        const Toast = Swal.mixin({
          toast: true,
          position: 'top-end',
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
          onOpen: (toast) => {
            toast.addEventListener('mouseenter', Swal.stopTimer)
            toast.addEventListener('mouseleave', Swal.resumeTimer)
          }
        })
        Toast.fire({
          icon: 'error',
          title: err
        })
      })
    },
    onSuccess (googleUser) {
      const profile = googleUser.getBasicProfile()
      const id_token = googleUser.getAuthResponse().id_token
      console.log(id_token);
      axios({
        method: 'POST',
        url: 'https://elixir-fox-kanban.herokuapp.com/googleLogin',
        data: {
          id_token
        }
      })
      .then((data)=>{
        console.log(data);
        localStorage.setItem('access_token', data.data.access_token)
        this.$emit('logged')
        const Toast = Swal.mixin({
          toast: true,
          position: 'top-end',
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
          onOpen: (toast) => {
            toast.addEventListener('mouseenter', Swal.stopTimer)
            toast.addEventListener('mouseleave', Swal.resumeTimer)
          }
        })
        Toast.fire({
          icon: 'success',
          title: 'Logged in successfully'
        })
      })
      .catch((err)=>{
        const Toast = Swal.mixin({
          toast: true,
          position: 'top-end',
          showConfirmButton: false,
          timer: 3000,
          timerProgressBar: true,
          onOpen: (toast) => {
            toast.addEventListener('mouseenter', Swal.stopTimer)
            toast.addEventListener('mouseleave', Swal.resumeTimer)
          }
        })
        Toast.fire({
          icon: 'error',
          title: err
        })
      })
    },
    onFailure (error) {
            console.log('Error :', error)
        }
  }
}
</script>
<style scoped>
:root {
 --input-padding-x: 1.5rem;
 --input-padding-y: .75rem;
}

.card-signin {
 border: 0;
 border-radius: 1rem;
 background: rgba(255, 255, 255, 0.5);
 box-shadow: 0 0.5rem 1rem 0 rgba(0, 0, 0, 0.1);
 text-align: center;
}

.card-signin .card-title {
 margin-bottom: 2rem;
 font-weight: 300;
 font-size: 1.5rem;
}

.card-signin .card-body {
 padding: 2rem;
}

.form-signin {
 width: 100%;
}

.form-signin .btn {
 font-size: 80%;
 border-radius: 5rem;
 letter-spacing: .1rem;
 font-weight: bold;
 padding: 1rem;
 transition: all 0.2s;
}

.form-label-group {
 position: relative;
 margin-bottom: 1rem;
}

.form-label-group input {
 height: 30px;
 border-radius: 2rem;
 background: rgba(248, 117, 117, 0.4);
 text-align: center;
}

.form-label-group>input,
.form-label-group>label {
 padding: var(--input-padding-y) var(--input-padding-x);
}

.form-label-group>label {
 position: absolute;
 top: 0;
 left: 0;
 display: block;
 width: 100%;
 margin-bottom: 0;
 /* Override default `<label>` margin */
 line-height: 1.5;
 color: #495057;
 border: 1px solid transparent;
 border-radius: .25rem;
 transition: all .1s ease-in-out;
}

.form-label-group input::-webkit-input-placeholder {
 color: transparent;
}

.form-label-group input:-ms-input-placeholder {
 color: transparent;
}

.form-label-group input::-ms-input-placeholder {
 color: transparent;
}

.form-label-group input::-moz-placeholder {
 color: transparent;
}

.form-label-group input::placeholder {
 color: transparent;
}

.form-label-group input:not(:placeholder-shown) {
 padding-top: calc(var(--input-padding-y) + var(--input-padding-y) * (2 / 3));
 padding-bottom: calc(var(--input-padding-y) / 3);
}

.form-label-group input:not(:placeholder-shown)~label {
 padding-top: calc(var(--input-padding-y) / 3);
 padding-bottom: calc(var(--input-padding-y) / 3);
 font-size: 12px;
 color: #777;
}

.social{
  margin-top: 10px;
  width: inherit;
  display: flex;
  justify-content: center;
}

.link {
  margin-top: 10px;
 text-align: center;
}
</style>
