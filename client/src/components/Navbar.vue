<template>
 <div class="navbar">
  <div class="title">
   <h2><a href="#" class="kanbanbtn" @click.prevent="home()">Kanban</a></h2>
  </div>
  <div class="nav-content">
   <a href="#" class="homebtn" @click.prevent="home()">Home</a>
   <a href="#" class="addbtn" @click.prevent="openAdd()">Add</a>
   <button class="btn btn-danger" @click.prevent="logout()">Logout</button>
  </div>
 </div>
</template>

<script>
export default {
  name: 'Navbar',
  methods: {
    logout () {
      this.$emit('logout')
    },
    openAdd () {
      this.$emit('add')
    },
    home () {
      this.$emit('home')
    }
  }
}
</script>

<style scoped>
.navbar{
 height: 50px;
 width: 100%;
 position: fixed;
 z-index : 10;
}

.navbar a{
 color: white;
 font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
 text-decoration: none;
}

.navbar .nav-content a {
 margin: 10px;
 font-weight: 400;
 font-size: 18px;
}

.title a{
 font-family: 'Sofia', cursive;
}

.nav-content{
 font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
 font-size: 20px;
}

.kanbanbtn:hover,
.homebtn:hover,
.addbtn:hover{
 color: teal;
}

button{
  background: rgba(255, 0, 0, 0.5);
  border: rgb(255, 0, 0);
}

</style>
