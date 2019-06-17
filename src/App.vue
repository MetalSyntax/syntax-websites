<template>
  <div id="app">
  <nav class="navbar navbar-dark bg-dark">
    <a href="#" class="navbar-brand">Syntax Web</a>
  </nav>
  <div class="container">
    <div class="row mt-5">
      <div class="col-sm-4">
        <div class="card">
          <div class="card-header">
            <h3>Add new Website</h3>
          </div>
          <div class="card-body">
          <!--Add Prevent with FireBase-->
            <form @submit="addWebsite">
              <div class="form-group">
                <input type="text" class="form-control" v-model="newWebsite.name" placeholder="Name">
              </div>
              <div class="form-group">
                <input type="text" class="form-control" v-model="newWebsite.author" placeholder="Author">
              </div>
              <div class="form-group">
                <input type="text" class="form-control" v-model="newWebsite.url" placeholder="URL">
              </div>
              <button type="submit" class="btn btn-dark">Save</button>
            </form>
          </div>
        </div>
      </div>
      <div class="col-sm-8 text-center">
      <div class="card">
        <div class="card-header">
          <h3>Websites List</h3>
        </div>
       <div class="card-body">
              <table class="table table-striped table-bordered">
                <thead>
                  <tr>
                    <th>Name</th>
                    <th>Author</th>
                    <th>Operations</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(website, w) in websites">
                    <td>
                      <a v-bind:href="website.url" target="_blank">{{website.name}}</a>
                    </td>
                    <td>
                      {{website.author}}
                    </td>
                    <td>
                      <button @click="removeWebsite(w)" class="btn btn-danger">
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
//Use to Firebase
/*import * as Firebase from 'firebase'
import config from './config';
let app = Firebase.initializeApp(config);
let db = app.database();
let websitesRef = db.ref('websites');*/

export default {
  name: 'App',
  /*firebase: {
    websites: websitesRef
  },*/
  data () {
    return {
      websites: [],
      newWebsite: {
        name: '',
        author: '',
        url: ''
      }
    }
  },
  mounted() {
    if (localStorage.getItem('websites')) {
      try {
        this.websites = JSON.parse(localStorage.getItem('websites'));
      } catch(e) {
        localStorage.removeItem('websites');
      }
    }
  },
  methods: {
     addWebsite() {
      // ensure they actually typed something
      if (!this.newWebsite) {
        return;
      }
      this.websites.push(this.newWebsite);
      this.newWebsite = '';
      this.saveWebsites();
    },
    removeWebsite(x) {
      this.websites.splice(x, 1);
      this.saveWebsites();
    },
    saveWebsites() {
      const parsed = JSON.stringify(this.websites);
      localStorage.setItem('websites', parsed);
    }
    // Use to Firebase
    /*addWebsite() {
      websitesRef.push(this.newWebsite);
      this.newWebsite.title = '';
      this.newWebsite.author = '';
      this.newWebsite.url = '';
    },
    deleteWebsite(website) {
      if(confirm('Are you sure delete it?')) {
        websitesRef.child(website['.key']).remove();
        toastr.success('Website removed');
      }*/
    }
  }
</script>

<style>
#app {
background: #0F2027;  /* fallback for old browsers */
background: -webkit-linear-gradient(to right, #2C5364, #203A43, #0F2027);  /* Chrome 10-25, Safari 5.1-6 */
background: linear-gradient(to right, #2C5364, #203A43, #0F2027); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
height:100%
}
body {
background: #0F2027;  /* fallback for old browsers */
background: -webkit-linear-gradient(to right, #2C5364, #203A43, #0F2027);  /* Chrome 10-25, Safari 5.1-6 */
background: linear-gradient(to right, #2C5364, #203A43, #0F2027); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
}
</style>
