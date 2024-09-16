<template>
  <div class="kanban">
    <div class="card">
      <div class="done" v-for="tag in tags" :key="tag">
        <h4>{{tag}}</h4>
        <div class="wrap">
          <KanbanCard @refresh="refresh" v-for="data in StatusCheck(tag)" :key="data.id" :data="data" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import KanbanCard from './KanbanCard'
export default {
  name: 'KanbanList',
  components: {
    KanbanCard
  },
  data () {
    return {
      kanban : [],
      tags: ['Backlog','Todo','On progress','Done']
    }
  },
  methods: {
    getKanban () {
      axios({
        method: 'GET',
        url: 'https://elixir-fox-kanban.herokuapp.com/kanban',
        headers: {
          access_token: localStorage.access_token
          }
      })
      .then((result)=>{
        this.kanban = result.data
      })
    },
    StatusCheck(status){      
      let datafilter=[]
      if(this.kanban != undefined){
        // for (let i in this.kanban) {
        //   if(this.kanban[i].tag == status){
        //     datafilter[i] = this.kanban[i]
        //   }
        // }
        if(this.kanban != undefined){
        datafilter= this.kanban.filter(function (item) { return item.tag == status.toLowerCase() })
        }
      }
        return datafilter
    },
    refresh () {
      this.getKanban()
    }
  },
  created () {
    this.getKanban()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.card{
  width: fit-content;
  background: none;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.done{
  width: 300px;
  min-height: 450px;
  margin: 10px;
}
h4{
  margin-left: 50px;
}

.wrap{
  overflow: auto;
  overflow-wrap: inherit;
  height: 550px;
  width: fit-content;
}

</style>
