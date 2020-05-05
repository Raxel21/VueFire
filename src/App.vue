<template>
  <div id="app">
    <nav class="navbar navbar-dark bg-primary">
      <a href="/" class="navbar-brand">VueJs Firebase Database</a>
    </nav>
    <div class="container">
      <div class="row mt-5">
        <div class="col-sm-4">
          <div class="card">
            <div class="card-header">Add A new Website</div>
            <div class="card-body">
              <form @submit.prevent="addWebsite()">
                <div class="form-group">
                  <input
                    type="text"
                    class="form-control"
                    v-model="newWebsite.name"
                    placeholder="Name"
                  />
                </div>
                <div class="form-group">
                  <input
                    type="text"
                    class="form-control"
                    v-model="newWebsite.author"
                    placeholder="Author"
                  />
                </div>
                <div class="form-group">
                  <input
                    type="text"
                    class="form-control"
                    v-model="newWebsite.url"
                    placeholder="URL"
                  />
                </div>
                <button type="submit" class="btn btn-primary">
                  Save
                </button>
              </form>
            </div>
          </div>
        </div>
        <div class="col-sm-8 text-center">
          <img src="./assets/logo.png" />
          <div class="card">
            <div class="card-header">
              <h3>Websites List</h3>
            </div>
            <div class="card-body">
              <table class="table table-striped table-borderer">
                <thead>
                  <tr>
                    <th>Name</th>
                    <th>Author</th>
                    <th>Operations</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="w in websites" :key="w.key">
                    <td>
                      <a v-bind:href="w.url" target="_blank">
                        {{ w.name }}
                      </a>
                    </td>
                    <td>
                      {{ w.author }}
                    </td>
                    <td>
                      <button class="btn btn-danger" @click="deleteWebsite(w)">
                        Delete
                      </button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Firebase from "firebase";
import config from "./config";
let app = Firebase.initializeApp(config);
let db = app.database();
let websitesRef = db.ref("websites");

import toastr from "toastr";
export default {
  name: "App",
  firebase: {
    websites: websitesRef
  },
  data() {
    return {
      websites: [],
      newWebsite: {
        name: "",
        author: "",
        url: ""
      }
    };
  },
  methods: {
    addWebsite() {
      websitesRef.push(this.newWebsite);
      toastr.success("Website Added");
      this.newWebsite.name = "";
      this.newWebsite.author = "";
      this.newWebsite.url = "";
    },
    deleteWebsite(site) {
      if (confirm("Are you sure you want to delete it?")) {
        websitesRef.child(site[".key"]).remove();
        toastr.success("Website Remove");
      }
    }
  }
};
</script>

<style>
#app {
  background: #23074d; /* fallback for old browsers */
  background: -webkit-linear-gradient(
    to right,
    #cc5333,
    #23074d
  ); /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(
    to right,
    #cc5333,
    #23074d
  ); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  height: 100vh;
}
</style>
