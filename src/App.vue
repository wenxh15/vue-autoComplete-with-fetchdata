<template>
  <div id="app">
    <input placeholder="autoComplete" @change="handleChange" v-model="value">
    <div :key="name" v-for="name in matchName">
      <p @click="handleClick(name)">{{name}}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      value: "",
      users: [],
      matchName: []
    };
  },
  created() {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then(res => res.json())
      .then(res => (this.users = res));
  },
  methods: {
    handleChange() {
      let match = [];
      if (this.value) {
        let temp = new RegExp(`^${this.value}`, "i");
        match = this.users.filter(user => temp.test(user.name));
      }
      let matchName = match.map(user => user.name);
      this.matchName = matchName;
    },
    handleClick(name) {
      this.value = name;
      this.matchName = [];
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
