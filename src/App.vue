<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App" />
  <h3>{{ counter }}</h3>
  <AppButton label="Increment" :handle-click="incrementCounter" />
  <AppButton label="Decrement" :handle-click="decrementCounter" />

  <h1>Users</h1>
  <ul>
    <li v-for="(user, index) in users" :key="index">
      <p> {{ user.name }} - {{ user.age }}</p>
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
        <label for="age">Age:</label>
        <input type="number" id="age" v-model.number="editedUser.age" />
      </div>
      <button type="submit">Save</button>
      <button type="button" @click="cancelEdit">Cancel</button>
    </form>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import AppButton from './components/AppButton.vue';

export default {
  name: 'App',
  components: {
    HelloWorld, AppButton
  },
  data() {
    return {
      counter: 0,
      users: [
        { name: "John Doe", age: 30 },
        { name: "Jane Smith", age: 25 },
        { name: "Bob Johnson", age: 40 },
      ],
      editingUser: null,
      editedUser: { name: "", age: null },
    };
  },
  methods: {
    incrementCounter() {
      this.counter++;
    },
    decrementCounter() {
      this.counter--;
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
      this.editedUser = { name: "", age: null };
    },
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

ul li {
  list-style: none;
  display: flex;
  justify-content: center;
  align-items: center;
}

ul li p {
  flex: 0.5
}
</style>
