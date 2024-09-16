<template>
  <div class="card">
    <div class="backlog">
      <div class="cards">
       <div class="cardtitle">
        <h4>{{data.title}}</h4>
       </div>
        <h6>{{data.description}}</h6>
        <br />
        <h6>By : {{data.User.email}}</h6>
        <br />
        <div class="card-panel">
          <button
            v-if="data.tag!=='backlog'"
            class="btn btn-dark btn-sm"
            @click.prevent="backward({data})"
          >
            <img src="../assets/back.png" />
          </button>
          <button class="btn btn-danger btn-sm" @click.prevent="del({data})">Delete</button>
          <button
            v-if="data.tag!=='done'"
            class="btn btn-dark btn-sm"
            @click.prevent="forward({data})"
          >
            <img src="../assets/next.png" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "KanbanCard",
  props: ["data"],
  methods: {
    forward(data) {
      let task = {
        title: data.data.title,
        description: data.data.description,
        tag: ""
      };
      if (data.data.tag == "backlog") {
        task.tag = "todo";
      } else if (data.data.tag == "todo") {
        task.tag = "on progress";
      } else if (data.data.tag == "on progress") {
        task.tag = "done";
      }
      axios({
        method: "PUT",
        url: "https://elixir-fox-kanban.herokuapp.com/kanban/" + this.data.id,
        headers: {
          access_token: localStorage.access_token
        },
        data: task
      })
        .then(result => {
          this.refresh();
          Swal.fire("Success!", "Task Successfully Forwarded!", "success");
        })
        .catch(err =>{
          Swal.fire("Failed", err, "error")
        })
    },
    backward(data) {
      let task = {
        title: data.data.title,
        description: data.data.description,
        tag: ""
      };
      if (data.data.tag == "todo") {
        task.tag = "backlog";
      } else if (data.data.tag == "on progress") {
        task.tag = "todo";
      } else if (data.data.tag == "done") {
        task.tag = "on progress";
      }
      axios({
        method: "PUT",
        url: "https://elixir-fox-kanban.herokuapp.com/kanban/" + this.data.id,
        headers: {
          access_token: localStorage.access_token
        },
        data: task
      })
        .then(result => {
          this.refresh();
          Swal.fire("Success!", "Task Successfully Backwarded!", "success");
        })
        .catch(err =>{
           Swal.fire("Failed", err, "error")
        })
    },
    del (data) {
     let id = data.data.id
     axios({
      method: 'DELETE',
      url:'https://elixir-fox-kanban.herokuapp.com/kanban/'+id,
      headers: {
          access_token: localStorage.access_token
        }
     })
      .then((result)=>{
        this.refresh();
          Swal.fire("Success!", "Task Successfully Deleted!", "success");
      })
      .catch(err =>{
            Swal.fire("Failed", err, "error")
      })
    },
    refresh() {
      this.$emit("refresh");
    }
  }
};
</script>

<style scoped>
.card {
  background: transparent;
  color: black;
  display: flex;
  flex-direction: row;
}
.cards {
  padding: 10px;
  width: 300px;
  min-height: 200px;
  background: rgba(255, 255, 255, 0.5);
  margin: 20px;
  border-radius: 15px;
}
.cardtitle{
 height: 50px;
 color: darkslategray;
}

button {
  margin: 2px;
}
</style>