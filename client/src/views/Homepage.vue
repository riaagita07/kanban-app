<template>
 <div class="homepage">
  <h4>Homepage</h4>
  <KanbanList />
 </div>
</template>
<script>
import KanbanList from '../components/KanbanList'
export default {
  name: 'Homepage',
  components: {
    KanbanList
  }
}
</script>
<style scoped>
.homepage {
 height: 100%;
 width: 100%;
 color: white;
 font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
</style>
