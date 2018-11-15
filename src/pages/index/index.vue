<template>
  <div class="container" @click="clickHandle('test click', $event)">
     <button open-type="getUserInfo" @getuserinfo="onGetUserInfo">点击获取用户信息</button>
    <div class="userinfo" @click="bindViewTap"> 
      <img class="userinfo-avatar" v-if="getUserData.avatarUrl" :src="getUserData.avatarUrl" background-size="cover" /> 
      <div class="userinfo-nickname">
        <card :text="getUserData.nickName"></card>
      </div>
    </div> 
    <button open-type="getUserInfo" @click="scan">扫码</button>
  </div>
</template>

<script>
import card from '@/components/card'

export default {
  components: {
    card
  },
  data () {
    return {
      motto: 'Hello World',
      userInfo: {},
      scanResult:{}
    }
  },
  computed:{
    getUserData(){
      return wx.getStorageSync('userInfo');
    }
  },
  methods: { 
    bindViewTap () {
      const url = '../logs/main'
      wx.navigateTo({ url })
    },
    onGetUserInfo () {  
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              wx.setStorageSync('userInfo',res.userInfo); 
            }
          })
        },
        fail: function () { 
          wx.showModal({
            title: '警告',
            content: '尚未进行授权，请点击确定跳转到授权页面进行授权。',
            success: function (res) {
              if (res.confirm) {
                console.log('用户点击确定')
                wx.navigateTo({
                url: '../tologin/tologin',
                })
              }
            }
          })
          } 
      })
    },
    clickHandle (msg, ev) {
      console.log('clickHandle:', msg, ev)
    },
    scan(){
      if (!this.getUserData) {
        return
      }
      let vm=this;
      wx.scanCode({
        success (res) {
          vm.addBook(res.result);
        }
      })
    }
  },
   async addBook(data){
    const res=await this.request('/weapp/addBook','post',data);  
    if (res.code===0&&res.data.title) {
      wx.showToast({
        title: `${res.data.title}添加成功`,
        icon: 'success',
        duration: 2000
      })
    }
  }, 
  request(url,methods,data){
    return new Promise((resolve,reject)=>{
      wx.request({
        url: url, 
        methods,
        data: data, 
        success (res) {
          if (res.data.code === 0) {
            resolve(res.data);
          }else{
            reject(res.data);
          }
        } 
      })
    })
  },
}
</script>

<style scoped>
.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.userinfo-nickname {
  color: #aaa;
}

.usermotto {
  margin-top: 150px;
}

.form-control {
  display: block;
  padding: 0 12px;
  margin-bottom: 5px;
  border: 1px solid #ccc;
}

.counter {
  display: inline-block;
  margin: 10px auto;
  padding: 5px 10px;
  color: blue;
  border: 1px solid blue;
}
</style>
