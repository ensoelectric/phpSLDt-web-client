<template>
  <div style="padding:10%">
    <v-container>
      <v-layout row class="padding">
        <v-flex xs12 sm6 offset-sm3>
          <v-card>
          <v-card-title class="headline pink primary-title">phpSLDt web client <v-spacer></v-spacer> <about/></v-card-title>
           
            <v-card-text class="mx-auto mt-0 pa-4 text-center secondary text-no-wrap">
              <v-container>  
                  <v-layout row>
                    <v-flex xs12>
                      <v-text-field label="Host" v-model="http.auth.host" type="url" outlined dense required>

                      </v-text-field>
                    </v-flex>
                  </v-layout>
                  
                  <v-layout row>
                    <v-flex xs12>
                      <v-text-field label="Username" v-model="http.auth.username" type="text" outlined dense required>

                      </v-text-field>
                    </v-flex>
                  </v-layout>
                  
                  <v-layout row>
                    <v-flex xs12>
                      <v-text-field label="Password" v-model="http.auth.password" type="password" outlined dense required>

                      </v-text-field>
                    </v-flex>
                  </v-layout>
                  
                  <v-layout row>
                    <v-flex xs12>
                      <v-btn  @click="auth()" class="accent-4 color" >
                        <v-icon>mdi-login-variant</v-icon>Sign in
                      </v-btn>
                    </v-flex>
                  </v-layout>        
              </v-container>
            </v-card-text>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
     <message :code="code" v-if="code > 0"></message>
  </div>
</template>
<script>
import axios from "axios";
import message from "@/components/HttpMessage";
import about from "@/components/About";

export default {
  name: "Auth",
  components: {
    message,
    about
  },
	props:{
    http_credentials: Object
	},
  data(){
    return {
        http: {
            auth:{
                host: '',
                username: '',
                password:''
            }
        },
        code: 0
    }
  },
  methods: {
    auth(){
        axios.options(this.http.auth.host + '/diagrams', {headers: {Authorization: 'Basic ' + btoa(this.http.auth.username + ':' + this.http.auth.password), Accept:"*/*"}})
          .then((response) => {
            if(response.status==200){
                this.$emit('auth', this.http.auth.host, this.http.auth.username, this.http.auth.password);
            }
            
            this.code = 401
            
          })
          .catch(() => {
            this.code = 401
          });
          this.code = 0
    }
  }
}
</script>

