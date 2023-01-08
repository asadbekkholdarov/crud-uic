<template>
  <div class="container">
    <h1>CRUD</h1>
    <form @submit.prevent="upload">
      <div class="">
        <input type="text" v-if="showIdInput" v-model="form.id" />
        <input
          v-model="form.first_name"
          placeholder="Name"
          type="text"
          required
        />
        <input
          v-model="form.last_name"
          placeholder="Lastname"
          type="text"
          required
        />
        <input v-model="form.email" placeholder="Email" type="email" required />
        <input
          v-model="form.avatar"
          placeholder="Image URL"
          type="text"
          required
        />
        <br />
        <button
          type="submit"
          v-if="!showIdInput"
          class="uk-button-primary uk-button"
        >
          Create
        </button>
        <button
          @click="update(form)"
          v-if="showIdInput"
          class="uk-button-default uk-button"
        >
          Update
        </button>
      </div>
    </form>
    <hr />
    <main class="">
      <div v-for="user in users" :key="user.id">
        <div class="card">
          <img :src="user.avatar" alt="" />
          <h3>
            {{ user.first_name }} <br />
            {{ user.last_name }}
          </h3>
          <p>{{ user.email }}</p>
          <div class="btns">
            <button
              @click="edit(user)"
              class="uk-button uk-button-primary uk-button-small"
            >
              Edit
            </button>
            <button
              @click="remove(user)"
              class="uk-button uk-button-danger uk-button-small"
            >
              Delete
            </button>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      users: [],
      form: {
        first_name: "",
        last_name: "",
        email: "",
        avatar: "",
        id: "",
      },
      showIdInput: false,
    };
  },
  methods: {
    getUsers() {
      axios
        .get("http://localhost:3000/data")
        .then((res) => (this.users = res.data));
    },
    upload() {
      axios
        .post("http://localhost:3000/data", this.form)
        .then(() => {
          this.form.first_name = "";
          this.form.last_name = "";
          this.form.email = "";
          this.form.avatar = "";
          this.getUsers();
          alert("Saving..., Click OK");
        })
        .catch((e) => console.log(e));
    },
    edit(user) {
      this.form.first_name = user.first_name;
      this.form.last_name = user.last_name;
      this.form.email = user.email;
      this.form.avatar = user.avatar;
      this.form.id = user.id;
      this.showIdInput = true;
    },
    update(form) {
      axios
        .put("http://localhost:3000/data/" + form.id, {
          first_name: this.form.first_name,
          last_name: this.form.last_name,
          email: this.form.email,
          avatar: this.form.avatar,
        })
        .then(() => {
          this.form.first_name = "";
          this.form.last_name = "";
          this.form.email = "";
          this.form.avatar = "";
          this.getUsers();
          this.showIdInput = !this.showIdInput;
          this.showIdInput = false;
          alert("Updating..., Click OK");
        })
        .catch((e) => console.log(e));
    },
    remove(form) {
      axios.delete("http://localhost:3000/data/" + form.id).then(() => {
        this.getUsers();
        this.showIdInput = false;
        alert("Deleted...");
      });
    },
  },
  created() {
    this.getUsers();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
main {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
}

form div {
  display: flex;
  flex-direction: column;
  align-items: center;
}

input {
  padding: 8px 15px;
  border-radius: 1px;
  border: 1px solid grey;
  margin: 1px;
}
.card {
  width: 280px;
  margin: 10px;
  padding: 10px;
  border: #2c3e50 1px solid;
  border-radius: 7px;
}

.card img {
  width: 170px;
  height: 170px;
  border-radius: 50%;
}

.card:hover {
  box-shadow: 0px 0px 2px 0.5px rgb(159, 157, 159);
  transition: 0.4s;
  cursor: pointer;
}

.btns {
  display: flex;
  justify-content: space-around;
  margin-bottom: 5px;
}
</style>
