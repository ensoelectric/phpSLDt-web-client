<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" mini-variant app clipped class="grey darken-4">
      
      <template #append>
        <div class="mb-5 d-flex justify-center align-center">
            <about></about>   
        </div>
        <div class="mb-5 d-flex justify-center align-center">     
          <v-btn icon @click="logout()">
            <v-avatar :size="26">
              <v-icon>mdi-logout-variant</v-icon>
            </v-avatar>
          </v-btn>
          
        </div>
      </template>
      
      </v-navigation-drawer>
      
        <v-main>
          <div style="background: url( 'images/background.png') center center; background-size: cover; height:100%">

            <div id="main-content">
              <component :is="windows.component" v-bind="{ 'endpoint': windows.endpoint, 'http_credentials': http.auth, 'id': windows.id }" :key="windows.title +''+ Date.now()" @setWindow="setWindow" @auth="auth" @logout="logout"></component>
            </div>
          </div>
        </v-main>
    
  </v-app>
</template>

<script>
import diagrams from "./components/Diagrams";
import diagram from "./components/Diagram";
import auth from "./components/Auth";
import about from "./components/About";


export default {
  name: "App",

  components: {
    diagrams,
    diagram,
    auth,
    about
  },

  data: () => ({
    http: {
      auth:{
      host: '',
      username: '',
      password:''
      }
    },
    windows: {
        component: 'auth',
    },
    drawer: false,
  }),
  created() {
    this.$vuetify.theme.dark = true;
  },
  methods: {
  
    setWindow: function(component='', endpoint, id){
      this.windows.endpoint = this.http.auth.host.trim() + '/' + endpoint
      this.windows.id = id
      
      this.windows.component = null
      this.windows.component = component
      
      this.drawer= true
    },
    logout(){
      this.http.auth.host = null
      this.http.auth.username = null
      this.http.auth.password = null
      this.windows.component="auth"
      this.drawer = false
    },
    auth(host, username, password){
      
      this.http.auth.host = host
      this.http.auth.username = username
      this.http.auth.password = password

      this.setWindow('diagrams','diagrams')
    } 
  }
};
</script>
