<template>
 <div class="addpage">
  <h3>Add Page</h3><br>
  <div class="form-add-page">
   <form>
    <div class="form-group">
     <input class="form-control" type="text" placeholder="Title" v-model="kanban.title" required>
    </div>
    <div class="form-group">
     <input class="form-control" type="text" placeholder="Description" v-model="kanban.description" required>
    </div>
    <div class="form-group">
     <button class="btn btn-dark" @click.prevent="add()">Add</button>
     <button class="btn btn-danger" @click.prevent="cancel()" >Cancel</button>
    </div>
   </form>
  </div>
 </div>
</template>

<script>
 import axios from 'axios'
 export default {
  name: 'Addpage',
  data () {
   return {
    kanban: {
     title: '',
     description: '',
     tag: 'backlog'
    }
   }
  },
  methods: {
   cancel () {
    this.$emit('cancel')
   },
   add () {
     axios({
      method: 'POST',
      url: 'https://elixir-fox-kanban.herokuapp.com/kanban',
      headers: {
       access_token : localStorage.access_token
      },
      data: {
       title : this.kanban.title,
       description: this.kanban.description,
       tag: this.kanban.tag
      }
     })
     .then((result)=>{
      this.cancel()
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
          title: 'Task Added successfully'
        })
     })
   }
  }
 }
</script>

<style scoped>

.addpage{
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: white;
  width: 100%;
}
.form-add-page{
 display: flex;
 justify-content: center;
 width: 100%;
 height: 400px;
 text-align: center;
}

form{
 width: 400px;
}

input[type="text"]{
 text-align: center;
}

input[type="text"]::placeholder {
 text-align: center;
 color: rgba(248, 117, 117, 0.7);
}

button {
 margin-right: 5px;
}
</style>