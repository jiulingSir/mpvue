<template>
  <div> 
    <todo-list></todo-list>
  </div>
</template>

<script> 
import TodoList from '@/components/todolist'

export default {
  components: {
    TodoList
  },

  data () {
    return {
      logs: [] 
    }
  },

  created () {
    const logs = (wx.getStorageSync('logs') || []) 
  }
}
</script>

<style>
.log-list {
  display: flex;
  flex-direction: column;
  padding: 40rpx;
}

.log-item {
  margin: 10rpx;
}
</style>
