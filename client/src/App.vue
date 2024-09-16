<template>
  <div id="app">
    <div class="header">
     <Navbar @home="home" @add="openAdd" @logout="logout" v-if="isLogin" />
    </div>
    <Addpage @cancel="home" v-if="isLogin && isAdd" />
    <Homepage v-else-if="isLogin && !isAdd" />
    <Loginpage @logged="logged" v-else-if="!isLogin" />
  </div>
</template>

<script>
import Homepage from './views/Homepage.vue'
import Loginpage from './views/Loginpage.vue'
import Addpage from './views/Addpage.vue'
import Navbar from './components/Navbar.vue'

export default {
  name: 'App',
  components: {
    Homepage,
    Loginpage,
    Navbar,
    Addpage
  },
  data () {
    return {
      isLogin: false,
      isAdd: false
    }
  },
  methods: {
    logged () {
      this.isLogin = true
    },
    logout () {
      this.isLogin = false
      localStorage.clear()
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
          title: 'Logged out successfully'
        })
      var auth2 = gapi.auth2.getAuthInstance();
      auth2.signOut().then(function() {
        console.log('User signed out.');
      });
    },
    openAdd () {
      this.isAdd = true
    },
    home () {
      this.isAdd = false
    }
  },
  created () {
    if (localStorage.access_token) {
      this.isLogin = true
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poiret+One&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Sofia&display=swap');


#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background: url('//hdwallpapers.imgix.net/freestockphotos-44901565544399i0xpir1orw.jpg?auto=compress&cs=tinysrgb&dpr=1');
  background-repeat: no-repeat;
  background-size: 100%;
  min-height: 695px;
  max-height: 100%;
}

.header{
  height: 50px;
}

::-webkit-scrollbar {
    display: none;
}
</style>
