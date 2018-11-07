<template>
  <div>
  	<p class="todolist-form">
  		<input type="text" class="todolist-input" v-model="text" name="">
  		<button size="mini" @click="submitEvent" class="todolist-button">添加</button>
  	</p>
  	 <ul class="todolist-list">
        <li v-for="(item, index) in lists" wx:key="index" :key="index">
          {{index+1}}:  {{item}} <span @click="deleteItem(index)">删除</span>
        </li>
    </ul>
  </div>
</template>

<script>  

export default { 
  data () {
    return { 
      lists:[],
      text:''
    }
  },

  created () { 
  },
  methods:{ 
  	submitEvent(){
         let isExist = this.lists.includes(this.text);
         if (isExist) {
         	return wx.showToast({
			  title: '当前值已存在', 
              icon: 'none',
			  duration: 2000
			})
         }
         this.lists.push(this.text);
  	},
  	deleteItem(index){
  		this.lists.splice(index,1);
  	}
  }
}
</script>

<style> 
.todolist-form{
	display:flex;
	margin:10px;
	justify-content:center;
}
.todolist-input{
	border:1px solid #ddd;
	width:100px;
	height:26px;
}
.todolist-button{
	margin:0;
}

.todolist-list{
	width: 50%;
	margin:0 auto;
}
</style>
