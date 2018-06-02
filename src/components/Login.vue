<template>
  <div>
    <form @submit.prevent="submit">
      <input type="url" placeholder="server" v-model="server"/><br/>
      <input type="text" placeholder="username" v-model="username"/><br/>
      <input type="password" placeholder="password" v-model="password"/><br/>
      <input type="submit" value="Login"/>
    </form>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      server: "http://localhost:5000",
      path: "/login",
      username: null,
      password: null,
      token: null
    };
  },
  methods: {
    submit() {
      const data = {
        EmailOrUsername: this.username,
        Password: this.password
      };
      fetch(this.server + this.path, {
        method: "POST",
        headers: {
          "content-type": "application/json"
        },
        body: JSON.stringify(data),
        mode: "cors"
      })
      .then(resp => resp.text())
      .then(token => this.$emit("login", this.server, token, this.username));
    }
  }
};
</script>

<style scoped>
</style>
