<template>
  <img alt="Vue logo" src="./assets/logo.png" width="100" height="100">
  <HelloWorld msg="Welcome to Your Vue.js App" />
  <h3>{{ counter }}</h3>
  <AppButton label="Increment" :handle-click="incrementCounter" />
  <AppButton label="Decrement" :handle-click="decrementCounter" />

  <h1>Users</h1>
  <form @submit.prevent="addUserToList" class="form">
    <TextField type="text" name="name" placeholder="name" v-model="user.name" />
    <TextField type="text" name="website" placeholder="website" v-model="user.website" />
    <AppButton label="Add user" type="submit" />
  </form>
  <h3 v-if="loading">Loading...</h3>
  <ul v-if="!loading">
    <li v-for="(user, index) in users" :key="index">
      <p> {{ user.name }} - {{ user.website }}</p>
      <AppButton :handle-click="() => editUser(index)" label="Edit" />
      <AppButton :handle-click="() => deleteUser(index)" label="Delete" />
    </li>
  </ul>

  <div v-if="editingUser !== null">
    <h2>Edit User</h2>
    <form @submit.prevent="saveUser">
      <div>
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="editedUser.name" />
      </div>
      <div>
        <label for="website">website:</label>
        <input type="text" id="website" v-model.number="editedUser.website" />
      </div>
      <button type="submit">Save</button>
      <button type="button" @click="cancelEdit">Cancel</button>
    </form>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import AppButton from './components/AppButton.vue';
import TextField from './components/TextField.vue';
import axios from "axios";

export default {
  name: 'App',
  components: {
    HelloWorld, AppButton, TextField
  },
  data() {
    return {
      counter: 0,
      loading: false,
      users: [],
      editingUser: null,
      editedUser: { name: "", website: "" },
      user: { name: "", website: "" },

    };
  },
  methods: {
    incrementCounter() {
      this.counter++;
    },
    decrementCounter() {
      this.counter--;
    },
    addUserToList() {
      const newUser = { name: this.user.name, website: this.user.website }
      this.users = [newUser, ...this.users]
      this.user.name = ""
      this.user.website = ""
    },
    editUser(index) {
      this.editingUser = index;
      this.editedUser = { ...this.users[index] };
    },
    deleteUser(index) {
      this.users.splice(index, 1);
    },
    saveUser() {
      this.users.splice(this.editingUser, 1, this.editedUser);
      this.cancelEdit();
    },
    cancelEdit() {
      this.editingUser = null;
      this.editedUser = { name: "", website: "" };
    },
  },
  mounted() {
    this.loading = true
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((response) => {
        this.loading = false
        this.users = response.data;
        console.log(response.data)
      })
      .catch((error) => {
        this.loading = false
        console.log(error);
      });
  },
}
</script>

<style >
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.form {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 2px;
}

ul li {
  list-style: none;
  display: flex;
  justify-content: center;
  align-items: center;
}

ul li p {
  flex: 0.5;
  text-align: left;
}
</style>
