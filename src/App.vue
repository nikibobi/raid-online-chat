<template>
  <div id="app">
    <Chat v-if="isAuthenticated" :server="server" :token="token" :username="username" />
    <Login v-else @login="login" />
  </div>
</template>

<script>
import Login from './components/Login.vue'
import Chat from './components/Chat.vue'

export default {
  name: 'app',
  components: {
    Login,
    Chat
  },
  data() {
    return {
      token: null
    }
  },
  methods: {
    login(server, token, username) {
      this.server = server;
      this.token = token;
      this.username = username;
    },
    logout() {
      this.token = null;
    }
  },
  computed: {
    isAuthenticated() {
      return this.token != null;
    }
  }
}
</script>

<style>
@font-face {
    font-family: Fontin;
    src: url(~/Fontin-Regular.ttf);
}

#app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
  padding-top: 25vh;
}
</style>
