<template>
  <div>
    <v-dialog v-model="dialog" width="85%">
        <template v-slot:activator="{ on, attrs }">
            <v-icon @click="del(index)" v-bind="attrs" v-on="on">mdi-lifebuoy</v-icon>
        </template>

       <v-card>
        <v-card-title class="text-h5 lighten-2">
         phpSLDt-web-client
        </v-card-title>
        <v-card-subtitle class="text-h5 lighten-2">
        version 0.1.2
        </v-card-subtitle>
        <v-card-text>
        <p>Artirm Pletnev <a href="mailto:artirm.pletnev@gmail.com">artirm.pletnev@gmail.com</a></p>
        <p>Клиент для работы с RESTful веб-сервисом <a href="https://github.com/ensoelectric/phpSLDt">phpSLDt</a>. Для дополнительной информации посетите <a href="https://github.com/ensoelectric/phpSLDt-web-client">https://github.com/ensoelectric/phpSLDt-web-client</a></p>
        
        <p>Исходный код клиента доступен на <a href="https://github.com/ensoelectric/phpSLDt-web-client">https://github.com/ensoelectric/phpSLDt-web-client</a></p>
        
        <p>Distributed under the MIT software license, see the accompanying file COPYING or <a href="https://github.com/ensoelectric/phpSLDt-web-client/blob/master/LICENSE">https://github.com/ensoelectric/phpSLDt-web-client/blob/master/LICENSE</a></p>    
        
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            text
            @click="dialog = false"
          >
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        dialog: false,
      }
    }
  }
</script>