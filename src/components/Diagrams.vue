<template>
	<div style="position: relative; background-color: rgba(18, 18, 18, 0.6);  min-height: 100vh;">
    <filter></filter>
    <v-toolbar :class="appBarColor" class="darken-3" flat :extended="isSearching" style="position: sticky; top: 0; z-index: 1; margin-right: 1px;">
    
      <v-toolbar-title>{{ appBarTitle }}</v-toolbar-title>
      
      <v-spacer></v-spacer>
      
      <v-btn icon @click="isSearching = !isSearching">
       <v-badge color="primary" v-if="search != null && search.length >=1" dot><v-icon>mdi-magnify</v-icon></v-badge>
       <v-icon v-else>mdi-magnify</v-icon>
      </v-btn>
       
      <v-btn icon @click="createDiagram">
        <v-icon>mdi-plus</v-icon>
      </v-btn>
      
      <template #extension v-if="isSearching">
      
        <v-text-field autofocus style="margin-bottom: -25px" class="mr-2" prepend-inner-icon="mdi-magnify" clearable rounded filled solo flat dense  @click:clear="search = null" @keydown.esc="isSearching = false, search = null" placeholder="Search" v-model="search">
        </v-text-field>
      </template>
    </v-toolbar>
    
    
    <v-list dense dark subheader two-line>
      <v-list-item-group>
      
        <v-subheader v-if="headers['x-total-count']">Найдено {{ headers['x-total-count'] }} схем</v-subheader>


        <v-list-item active-class="highlighted" link v-for="(issue, index) in body" :key="index" @click="select('diagram', 'diagrams/'+issue.id, issue.id)">
      <v-list-item-avatar>
            <v-icon>mdi-transmission-tower</v-icon>
          </v-list-item-avatar>
          
          <v-list-item-content>
            <v-list-item-title> {{`${issue.label}` }}</v-list-item-title>
            <v-list-item-subtitle><strong>Created: {{issue.created_at}}</strong> Last update: {{issue.updated_at}}</v-list-item-subtitle>

            {{ issue.location}}
          </v-list-item-content>
          
           <v-list-item-action>
          <v-btn icon>
            <v-icon color="grey lighten-1">mdi-chevron-right</v-icon>
          </v-btn>
        </v-list-item-action>
        </v-list-item>
          
      </v-list-item-group>
  
      <infinite-loading :identifier="infiniteId" @infinite="infiniteHandler">
        <div slot="spinner">
          <v-progress-circular indeterminate color="primary"></v-progress-circular>
        </div>
        <div slot="no-more">Показаны все результаты</div>
        <div slot="no-results">Ничего не найдено</div>
      </infinite-loading>
    </v-list>
    <message :code="code" v-if="code > 0"></message>
	</div>
</template>


<script>
import axios from "axios";
import message from "@/components/HttpMessage";
import InfiniteLoading from 'vue-infinite-loading';

export default {
  name: "Diagrams",
  components: {
    InfiniteLoading,
    message
  },
	props:{
    http_credentials: Object
	},
  data(){
    return {
      appBarColor: "pink",
      appBarTitle: "Однолинейные схемы",
      isSearching: false,
      search: "",
      body: [],
      headers: {},
      infiniteId: +new Date(),
      links: [],
      types: [],
      cancelSource: null,
      code: 0
      
    }
  },
  watch:{
    search(){
      this.body = []
      this.headers = {}
      this.links = []
      this.infiniteId= +new Date()
    } 
  },
  methods: {
    select(component, endpoint, id){
		this.$emit('setWindow', component, endpoint, id);

    },
    refresh: function () {
        this.body = []
          axios.get(this.http_credentials.host + '/diagrams', {headers: {Authorization: 'Basic ' + btoa(this.http_credentials.username + ':' + this.http_credentials.password), Accept: '*/*'}})
              .then(response => {
                this.body = response.data
                this.headers = response.headers
              })
              .catch((error) => {
                this.code = error.response.status
              })
    },
    createDiagram(){
        axios.post(this.http_credentials.host + '/diagrams', '{ "label": "New single line diagram" }', {headers: {Authorization: 'Basic ' + btoa(this.http_credentials.username + ':' + this.http_credentials.password), Accept: "*/*"}})
          .then((response) => {
            this.code = response.status
            this.refresh()
          })
          .catch((error) => {
            if (error.response.status === 401) {
                //logout
            }
            this.code = error.response.status

          });
        this.code = 0
    },
    buildLinks(links){
      if (typeof(links) != 'string') return 0
      
      let parts = links.split(',');

      // Parse each part into a named link
        for(let i=0; i<parts.length; i++) {
            let section = parts[i].split(';');
            if (section.length !== 2) {
                        throw new Error("section could not be split on ';'");
                    }
            let url = section[0].replace(/<(.*)>/, '$1').trim();
            let name = section[1].replace(/rel="(.*)"/, '$1').trim();

            this.links[name] = url;
			}
      
		},
    infiniteHandler($state) {

      if (this.cancelSource) this.cancelSource.cancel()

      this.cancelSource = axios.CancelToken.source()

      let endpoint = this.http_credentials.host+'/diagrams'   
      
      if(this.search != null && this.search.length >=1)
        endpoint = this.http_credentials.host+'/diagrams?search='+this.search
      
      if(this.links.next) endpoint = this.links.next
      
      if (!this.links.current || this.links.current !== this.links.last) { 
        axios.get(endpoint,
            {
              cancelToken: this.cancelSource.token,
              headers: {Authorization: 'Basic ' +btoa(this.http_credentials.username+':'+this.http_credentials.password), Accept: '*/*'}
            }
        )
        .then((response) => {
          this.body.push(...response.data)
          this.headers = response.headers
          this.buildLinks(this.headers["link"])
          this.cancelSource = null
          $state.loaded();
        })
        .catch((error) => {
          $state.complete();
          console.log("ERROR, ", error.message)
        });
      } else {
            $state.complete();
          }
      console.log("Headers,", this.headers["link"])
    }

  },
  mounted(){
    //console.log("Body,", this.body)
    //console.log("Headers,", this.headers)
  }
};
</script>
