<template>
  <div>
    <v-dialog v-model="dialog" width="500">
        <template v-slot:activator="{ on, attrs }">
            <v-icon @click="del(index)" v-bind="attrs" v-on="on">mdi-close</v-icon>
        </template>

        <v-card>
            <v-toolbar color="red darken-3" dark>
            <v-toolbar-title>Confirm</v-toolbar-title>
            </v-toolbar>
    
            <v-card-text>
                <div class="pt-6"><slot>Удалить? Данное действие отменить будет невозможно.</slot></div>
            </v-card-text>
            
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="red lighten-1" dense small @click="yes"> Да, удалить</v-btn>
                <v-btn color="primary" dense small @click="dialog = false"> Отмена</v-btn>
            </v-card-actions>
        </v-card>		
    </v-dialog>
  </div>
</template>

<script>
  export default {
    props:{
        id: Number
    },
    data () {
      return {
        dialog: false,
      }
    },
    methods:{
        yes(){
            this.$emit('delete', this.id)
            this.dialog = false
        }
    }
  }
</script>