<template>
  <div class="root">
    <div class="header">
    <p>
      <img class="logo" src="../images/logo.jpg" alt="ロゴ">

    </p>
    {{reactive}}
    <div class='form'>
    <div class='form-rename'>
    <p>ユーザ名<p/>
    <form @submit="rename" >
      <input v-model="name" type="text">
      <button type="submit">変更</button>
    </form>
    </div>

    <div class="form-msg">
    <p>テキスト<p/>
    <form @submit="onSubmit">
      <input v-model="$data.text" type="text">
      <button type="submit">送信</button>
    </form>
    </div>
    </div>

    </div>

<div class="container">
  <div class="sidebar">
    <div class="sidebar-sticked">
      <p>参加者リスト</p>
    <ul>
      <template v-for="user in userList">
        <li v-show="!(user.name==='管理者')"
            v-bind:class="{deactive: !user.active}"
            class='sidebar-user'>
          {{user.name}}
          </li>
      </template>
    </ul>
    </div>
  </div>

  <div class='chat'>
    <div class = 'chat-margin'>
    <MyComponent v-for="msg in message"
    :key ="msg.id"
    :message="msg.text"
    :name="$data.userList[msg.socketid].name"
    :active ="$data.userList[msg.socketid].active"
    :time ="msg.time"
    :class="{me:$data.socketid==msg.socketid,
            other:!($data.socketid==msg.socketid)}"
    />
    </div>
  </div>
</div>

  </div>
</template>

<script>
import socket from './utils/socket';

// components
import MyComponent from './components/MyComponent.vue';

export default {
  components: {
    MyComponent
  },
  data() {
    return {
    // userList: { { socketid: {name:string, active: bool } }, ... }
      message: [],
      text: '',
      count: 0, // v-forのkey用
      userList: { 'admin': { name: '管理者', active: true }},
      name: '',
      socketid: '',
      reactive: '',
    };
  },
  created() {
    socket.on('connect', () => {
      console.log('connected!!!!');
      const user = { name: socket.id, acrtive: true };
      // this.$data.userList[socket.id] = user;
      this.$set(this.$data.userList, socket.id, user);
      this.$data.name = socket.id;
      this.$data.socketid = socket.id;
      socket.emit('addUser', socket.id);
    });

    socket.on('send', (data) => {
      // console.log(message);
      data.id = this.$data.count;
      this.$data.count++;
      this.$data.message.push(data);
    });

    socket.on('rename', (data) => {
      console.log(this.userList);
      this.$data.userList[data.socketid].name = data.name;
      // 再描画
      this.$data.reactive = ' ';
    });

    socket.on('addUser', (data) => {
      if (data !== socket.id) {
        const user = { name: data, active: true };
        this.$set(this.$data.userList, data, user);
        socket.emit('sendProfile', {
          socketid: socket.id,
          name: this.$data.name
        });
      }
    });

    socket.on('deleteUser', (socketid) => {
      this.$data.userList[socketid].active = false;

      const msg = {};
      msg.id = this.$data.count;
      this.$data.count++;
      msg.text = this.$data.userList[socketid].name + 'さんが退出しました。';
      msg.socketid = 'admin';
      msg.time = this.getTime();
      this.$data.message.push(msg);
    });

    socket.on('sendProfile', (data) => {
      this.$data.userList[data.socketid] = { name: data.name, active: true };
      // 再描画
      this.$data.reactive = ' ';
    });
  },
  methods: {
    /**
     * Enterボタンを押したとき
     */
    onSubmit(e) {
      e.preventDefault();
      if (this.$data.text) {
        socket.emit('send', {
          text: this.$data.text,
          time: this.getTime()
        });
        this.$data.text = '';
      }
    },

    rename(e) {
      e.preventDefault();
      socket.emit('rename', {
        name: this.$data.name,
        socketid: socket.id
      });
    },

    getTime() {
      const DD = new Date();
      const Year = DD.getFullYear();
      const Month = DD.getMonth() + 1;
      const Day = DD.getDate();
      const Hour = DD.getHours();
      const Minute = DD.getMinutes();
      const Second = DD.getSeconds();
      return (Year + '/' + Month + '/' + Day + ' ' + Hour + ':' + Minute + ':' + Second);
    }
  }
};
</script>

<style lang="scss" scoped>
.root {
  overflow-wrap: break-word;
  font-family: serif;
}

ul {
  list-style: none;
  padding-left: 0;
}

.logo {
  width: 40px;
  margin-right: 50px;
}

.sample {
  color: $red;
}

.header {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 1;
  background-color: #fff;
  min-width: 100vw;
}

.form {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.form-rename {
  margin-right: 20px;
}

.container {
  display: flex;
  justify-content: center;
  min-height: 100vh;
  padding-top: 20px;
}

.sidebar {
  background-color: #a0a0a0;
}

.sidebar-sticked {
  position: -webkit-sticky;
  position: sticky;
  top: 100px; //上のフォーム分空ける
}

.sidebar-sticked ul::after {
  content: '';
  border-top: solid 1px;
  display: block;
}

.sidebar-user {
  border-top: solid 1px;
  padding: 5px 0;
}

.deactive {
  opacity: 0.3;
}

.chat {
  width: 600px;
  background-color: #71beeb;
}

.chat::before {
  content: '';
  display: block;
  height: 100px; //上のフォーム分の高さ
}

.chat-margin {
  margin-left: 15px;
  margin-right: 15px;
}

.me {
  text-align: right;
  margin-left: 50%;
}

.other {
  margin-right: 50%;
}
</style>
