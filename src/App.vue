<template>
  <div id="app" style="background-color: #e6ecf0;">

    <nav class="navbar navbar-toggleable-md navbar-light fixed-top bg-faded">
      <button class="navbar-toggler navbar-toggler-right" type="button" data-toggle="collapse" data-target="#navbarCollapse" aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarCollapse">
        <div class="container">
          <ul class="navbar-nav mr-auto">
            <li class="nav-item active">
              <a class="nav-link nav-color" href="#">Inicio <span class="sr-only">(current)</span></a>
            </li>
            <img class="img-logo" src="./assets/logo.png" width="40px" height="35px">
          </ul>
        </div>
      </div>
    </nav>
    
    <div class="container" style="padding-top: 70px;">
      <div class="row">
        <div class="col-sm-4" style="background-color: #1DA1F2; margin-right:30px;">
          <img src="./assets/user.png" class="img-user" width="200px" height="200px"/>
          <div class="text-user">
            <h3>Luis Toro</h3>
            <p>@LUISTOROG</p>
          </div>

          <div class="position-create">
            
            <form>
              <div class="form-group">
                <label for="contentInput"><b>Content</b></label>
                <textarea class="form-control" id="contentInput" placeholder="Enter content" rows="3" v-model="tweet.content" required/>
              </div>

              <div class="form-group">
                <label for="locationInput"><b>Location</b></label>
                <input type="text" class="form-control" id="locationInput" placeholder="Enter location" v-model="tweet.location" required/>
              </div>

              <div class="form-group">
                <label for="authorSelect"><b>Author select</b></label>
                <select class="form-control" id="authorSelect" v-model="tweet.author" required>
                  <option v-for="(author, key) in authors" :key="key" :value="author._id">
                   {{ author.firstname }} {{ author.lastname }}
                  </option>
                </select>
              </div>

              <button type="button" class="btn btn-primary btn-lg btn-block"  @click.prevent="create">Post Tweet</button>

            </form>
          </div>


        </div>
        <div class="col-sm" style="background-color: white;">
          <h4 class="position-anuncio">Tweets</h4>
          <div style="background-color: white;">
            <ul>
              <div class="list-group">
                <li style="margin-bottom: 10px;" v-for="tweet in tweets" :key="tweet._id">
                  <a href="#" class="list-group-item list-group-item-action flex-column align-items-start">
                    <div class="d-flex w-100 justify-content-between">
                      <h5 class="mb-1">{{ tweet.author.firstname }} {{ tweet.author.lastname }}</h5>
                      <small>{{tweet.createdAt}}</small>
                    </div>
                    <div class="content-tweet w-100" style="margin-top: 10px; margin-bottom: 10px;">
                      <p class="mb-1" style="padding: 10px;">{{ tweet.content }}</p>
                    </div>                    
                    <small>{{ tweet.location }}</small>
                  </a>
                </li>
              </div>
            </ul>
          </div>
          <br>
        </div>
      </div>
    </div>
    
  </div>
</template>

<script>
import axios from "axios";
import config from "./config.json";

export default {
  name: 'app',
  data () {
    return {
      loading: true,
      tweets: [],
      tweet: {
        content: '',
        location: '',
        author: ''
      },
      authors: []
    }
  },
  created(){
    this.loadTweets();
    this.loadAuthors();
  },
  methods: {
    loadTweets(){
      axios.get(`${config.baseURL}/tweets?limit=20`,{
        withCredentials: false
      })
      .then(response => {
        this.loading = false;
        this.tweets = response.data.data;
      })
    },
    loadAuthors(){
      axios.get(`${config.baseURL}/users`,{
        withCredentials: false
      })
      .then( response => {
        this.authors = response.data.data;
      })
    },
    create(){
      axios.post(`${config.baseURL}/tweets`, this.tweet, {
        withCredentials: false,
        headers: {
          'Content-Type': 'application/json'
        }
      })
      .then( response => {
        this.loadTweets();
      })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  /*margin: 0 10px;*/
}

a {
  color: #42b983;
}

.img-user {
  display: block;
  margin-top: 25px;
  margin-left: auto;
  margin-right: auto;
  text-align: center;
}

.img-logo{
  display: block;
  margin-left: auto;
  margin-right: auto;
  text-align: center;
}

.text-user{
  margin-top: 20px;
  background-color: white;
  border-radius: 20px 20px;
}

.text-user h3{
  padding-top: 8px;
}

.text-user p{
  padding-bottom: 8px;
}

.text-user h3, .text-user p {
  text-align: center;
}

.nav-color{
  border-bottom: 2px solid #1c94e0;
}

.navbar-light .navbar-nav .active>.nav-link, .navbar-light .navbar-nav .nav-link.active, .navbar-light .navbar-nav .nav-link.open, .navbar-light .navbar-nav .open>.nav-link {
    color:#1DA1F2 !important;
} 

.content-tweet{
  background-color: #1DA1F2;
  border-radius: 5px 5px;
  
}

.content-tweet p{
  color: white;
}

.position-anuncio{
  text-align: center;
  margin-top: 17px;
  margin-bottom: 25px;
}

.position-create{
  padding: 15px;
  background-color: white;
  border-radius: 5px 5px;
}

</style>
