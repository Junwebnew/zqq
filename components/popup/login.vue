<template>
  <div class="baseZoom zoomTrue" :class="{'zoom': isFixed}" v-if="isShow">
    <transition name="fadeIn">
      <div class="content confirm login" >
        <em class="icon-close" @click="close"></em>
        <div v-if="type == 'register'" class="title">{{title}}</div>
        <div v-if="type == 'login'" class="title">{{title}}</div>
        <dl>
          <dt>
            手机号
          </dt>
          <dd v-if="type == 'register'">
            <input type="text" @keyup.enter="register" placeholder="请输入手机号" maxlength="11" v-model="phone">
          </dd>
          <dd v-if="type == 'login'">
            <input type="text" @keyup.enter="login" placeholder="请输入手机号" maxlength="11" v-model="phone">
          </dd>
        </dl>
        <dl v-if="type == 'register'">
          <dt>
            联系人
          </dt>
          <dd>
            <input type="text" placeholder="请输入您的姓名" maxlength="14" v-model="name">
          </dd>
        </dl>
        
        <a  href="javascript:void(0);" @click="login" class="btn">提交</a>
      </div>
    </transition>
  </div>
</template>
<script>
import popup from "./index.js"
import axios from "axios"
export default {
  data() {
    return {
      isFixed: true,
      isRead: true,
      isShow: true,
      icon: undefined,
      type: "",
      name: '',
      phone: '',
      cancel: true
    };
  },
  created() {},
  mounted() {
    if (this.time) {
      t && clearTimeout(t);
      let t = setTimeout(() => {
        this.close();
      }, this.time);
    }
  },
  methods: {
    close() {
      this.isShow = false;
    },
    showTab (type) {
      this.type = type
    },
    saveLoginInfo (requireObj) {
        // axios.post("/server/loginSession", requireObj).then(res => {
        //     //window.history.go(0)
        // }).catch((err) => {
        //     console.log(err)
        // });
    },
    register () {
      if (!this.isRead) {
        popup.created({
            content: '请阅读用户协议',
            time: 2000
        })
        return false
      }
      if (!this.name) {
        popup.created({
            content: '请输入姓名',
            time: 2000
        })
        return false
      }
      if (!this.phone) {
        popup.created({
            content: '请输入电话',
            time: 2000
        })
        return false
      }
      if (!/^1(3|4|5|6|7|8|9)\d{9}$/.test(this.phone)) {
        popup.created({
            content: '请输入正确电话',
            time: 2000
        })
        return false
      }
      this.success(this.name, this.phone, (res) => {
        this.isShow = false
        this.saveLoginInfo({
                    nickname: res.data.nickname,
                    userId: res.data.userId,
                    phoneNo: res.data.phoneNo,
                    time: 7})
      });
    },
    login () {
      if (!this.name) {
        popup.created({
            content: '请输入您的姓名',
            time: 2000
        })
        return false
      }
      if (!this.phone) {
        popup.created({
            content: '请输入电话',
            time: 2000
        })
        return false
      }
      if (!/^1(3|4|5|6|7|8|9)\d{9}$/.test(this.phone)) {
        popup.created({
            content: '请输入正确电话',
            time: 2000
        })
        return false
      }
      this.loginFn(this.name, this.phone, (res) => {
        this.isShow = false
      });
    }
  }
};

</script>
<style scoped>
.baseZoom {
  position: fixed;
  z-index: 999;
  top: 30%;
  width: 200px;
  left: 50%;
  margin-left: -100px;
}

.zoom {
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  width: 100%;
  margin: 0;
  z-index: 999;
  line-height: 100%;
  text-align: center;
}
.zoomTrue{
  background: rgba(0,0,0,.5);
}

.baseZoom .content.w100 {
  width: 80px;
  margin-left: -40px;
}

.baseZoom .content {
  box-sizing: content-box;
  background: rgba(0, 0, 0, 0.8);
  display: block;
  padding: 20px;
  width: 200px;
  border-radius: 5px;
  vertical-align: middle;
  line-height: 30px;
  z-index: 102;
  left: 50%;
  margin-left: -100px;
  position: fixed;
  top: 30%;
  text-align: center;
  color: #fff;
}

.baseZoom .content i {
  font-size: 40px;
  margin-top: 10px;
  display: inline-block;
}

@keyframes loading {
  0% {
    transform: rotate(0deg);
  }

  50% {
    transform: rotate(180deg);
  }

  100% {
    transform: rotate(360deg);
  }
}
.baseZoom .login .icon-close {
  display: block;
  width: 23px;
  height: 23px;
  position: absolute;
  top: 4px;
  right: 4px;
  background: url("../../assets/images/main/close.png") 50% no-repeat;
  cursor: pointer;
}
.baseZoom .login{
  overflow: hidden;
  background: #fff;
  width: 353px;
  padding: 20px 50px 30px;
  color: #333;
  margin-left: -216px!important;
}
.baseZoom .login .title{
  padding: 0 0 25px 0;
  text-align: center;
  font-size: 24px;
  font-weight: bold;
}
.baseZoom .login .btn{
  margin-top: 25px;
}
.baseZoom .login dl{
  font-size: 14px;
  line-height: 40px;
  margin-bottom: 10px;
  overflow: hidden;
}
.baseZoom .login dl dt{
  float: left;
  width: 70px;
  text-align: right;
}
.baseZoom .login dl dd{
  padding-left: 10px;
  overflow: hidden;
  box-sizing: border-box;
}
.baseZoom .login dl dd input{
  width: 80%;
  height: 40px;
  line-height: 40px;
  border: 1px solid #eee;
  text-indent: 10px;
}
.tips{
  padding-top: 5px;
  color: #ff6600;
  cursor: pointer;
}

.btn{width:220px;height:50px;line-height:50px;text-align:center;color:#fff;background:#ff6600;font-size:16px;display:block;margin:0 auto;border-radius:4px;box-shadow:0 0 15px 5px rgba(28,139,238,.2);}
a.btn{color:#fff}
</style>
