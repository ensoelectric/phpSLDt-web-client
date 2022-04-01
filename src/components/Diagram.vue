<template>

  <div style="position: relative; background-color: rgba(18, 18, 18, 0.6);  min-height: 100vh;">
      <v-app-bar dark color="pink">
      <v-btn icon>
        <v-icon @click="select('diagrams', 'diagrams')">mdi-chevron-left</v-icon>
      </v-btn>
      <v-toolbar-title>Принципиальная схема групповой сети. ГОСТ 21.613-2014</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-menu bottom left>
            <template v-slot:activator="{ on, attrs }">
              <v-btn dark icon v-bind="attrs" v-on="on">
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
            </template>

            <v-list>
              <v-list-item v-for="(item, i) in menu" :key="i" link  @click="menuAction(item.action)">
                 <v-list-item-icon>
                    <v-icon>{{ item.icon }}</v-icon>
                </v-list-item-icon>
                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-list-item>
            </v-list>
          </v-menu>
          
    </v-app-bar>

    <v-container>

      <v-row>
        <v-col xl=4 lg=4 md=12 sm=12 xs=12>
          <v-card color="#385F73" dark>
          <v-system-bar window dark>
            <v-icon>mdi-information-variant</v-icon>
            <span>Данные распределительного устройства</span>
          </v-system-bar>
            <v-card-text>    
                <v-text-field label="Распределительное устройство"  v-model="body.label" @change="update" outlined></v-text-field>
                <v-text-field label="Марка оболочки распред. устройства"   v-model="body.enclosure.model" @change="update" outlined></v-text-field>
                <v-text-field label="Код оболочки распред. устройства"    v-model="body.enclosure.article" @change="update" outlined></v-text-field>
                <v-text-field label="Способ монтажа"    v-model="body.enclosure.construction" @change="update" outlined></v-text-field>
                <v-text-field label="Степень защиты по ГОСТ14.254-96"    v-model="body.enclosure.protection" @change="update" outlined></v-text-field>
                <v-textarea outlined v-model="body.location" label="Место установки распред. устройства" @change="update"></v-textarea>
                <v-text-field label="Количество фаз питания распред. устройства"   v-model="body.phases" @change="update" outlined></v-text-field>
                <v-text-field label="Тип питающей сети"   v-model="body.ground" @change="update" outlined></v-text-field>
                <v-text-field label="Электропитание осуществляется от распределительного устройства" v-model="body.supplier.label"  @change="update" outlined></v-text-field>
                <v-text-field label="Количество модулей по 17,5 мм, устанавливаемых в распределительном устройстве" v-model="body.enclosure.modules"  @change="update" outlined></v-text-field>   
            </v-card-text>
          </v-card>
        </v-col>
        
        <v-col xl=4 lg=4 md=12 sm=12 xs=12>
          <v-card color="#1F7087" dark>
          <v-system-bar window dark>
            <v-icon>mdi-transmission-tower-import</v-icon>
            <span>Аппарат до ввода в распределительное устройство</span>
          </v-system-bar>
            <v-card-text>    
                <v-text-field label="Тип аппарата"  v-model="body.supplier.device.device" @change="update" outlined></v-text-field>
                <v-text-field label="Номинальный ток, А"  v-model="body.supplier.device.rating" @change="update" suffix="A" outlined></v-text-field>
                <v-text-field label="Уставка расцепителя, А"  v-model="body.supplier.device.trip_settings" @change="update" suffix="A" outlined></v-text-field>
                <v-text-field label="Предельная коммутационная стойкость, кА"  v-model="body.supplier.device.interrupting_rating" @change="update" suffix="kA"  outlined></v-text-field>
                <v-text-field label="Тип защитной характеристики"    v-model="body.supplier.device.type" @change="update"  outlined></v-text-field>
                <v-text-field label="Количество отключаемых полюсов аппарата"   v-model="body.supplier.device.poles" @change="update"  outlined></v-text-field>
                <v-text-field label="Уставка дифференциального тока, мA"   v-model="body.supplier.device.leakage_cu10rrent_settings" @change="update" suffix="mA"  outlined></v-text-field>
                <v-text-field label="Обозначение"   v-model="body.supplier.device.label" @change="update"  outlined></v-text-field>
                 <v-textarea outlined  v-model="body.supplier.cable" label="Информация о кабеле" placeholder="Информация о кабеле, которым запитано данное распред. устройтво приведена в схеме распред. устройства, осуществляющего электропитание" @change="update"></v-textarea>
            </v-card-text>
          </v-card>
        </v-col>
        
        <v-col xl=4 lg=4 md=12 sm=12 xs=12>
          <v-card color="#952175" dark>
          <v-system-bar window dark>
            <v-icon>mdi-transmission-tower-export</v-icon>
            <span>Данные об итоговых значениях нагрузок распред. устройства</span>
          </v-system-bar>
            
            <v-card-text>    
                <v-text-field label="Установленная полная мощность распределительного устройства, кВА"  v-model="body.load.installed.capacity" @change="update" suffix="kVA" outlined></v-text-field>
                <v-text-field label="Ток от установленной мощности, А"  v-model="body.load.installed.current" @change="update" suffix="A" outlined></v-text-field>
                <v-text-field label="Расчетная полная мощность распределительного устройства, кВА"  v-model="body.load.estimated.power"  @change="update" suffix="kVA" outlined></v-text-field>
                <v-text-field label="Расчетный ток от эквивалентной группы трехфазных электропотребителей с суммарной мощностью однофазных электропотребителей, А"  v-model="body.load.estimated.current" @change="update" suffix="A" outlined></v-text-field>
                <v-text-field label="Усредненный коэффициент спроса"    v-model="body.load.demand_factor" @change="update"  outlined></v-text-field>
                <v-text-field label="Ток от установленной мощности в фазе А, А"   v-model="body.load.installed.current_a" @change="update" suffix="A" outlined></v-text-field>
                <v-text-field label="Ток от установленной мощности в фазе B, А"   v-model="body.load.installed.current_b" @change="update" suffix="A" outlined></v-text-field>
                <v-text-field label="Ток от установленной мощности в фазе C, А"   v-model="body.load.installed.current_c" @change="update" suffix="A" outlined></v-text-field>
            </v-card-text>
          </v-card>
        </v-col>
        
      </v-row>
      
        <v-row>
            <draggable v-model="body.applications" @end="updateapp" style="width:100%;">
                <transition-group>
                    <v-col cols="12" v-for="(issue, index) in body.applications" :key='index+1'>
                        <v-card color="#1F7087" dark>
                            <v-system-bar window dark @dblclick="expand(index)" >
                                <v-icon>mdi-electric-switch</v-icon>
                                <span>{{ issue.device }} <b>{{ issue.desc }}</b></span>
                                <v-spacer></v-spacer>
                                <v-icon v-if="expanded.includes(index)" @click="expand(index)">mdi-minus</v-icon>
                                <v-icon v-else @click="expand(index)">mdi-checkbox-blank-outline</v-icon>
                                <qdelete :id="index" @delete="del(...arguments)"/>
                            </v-system-bar>
                            
                            <v-slide-y-transition>
                                <v-card-text v-show="expanded.includes(index)">   
                                    <v-row align="center" class="mx-0">
                                        <v-col cols="6">
                                            <v-text-field label="Аппарат отходящей линии"  v-model="issue.device" hint="Примеры: QS, EATON IS-32/2, 32 A, 2P или DQ1, EATON PF6-40/2/003, AC, 30 мА, 1P+N, 40 A или 1QF3, EATON PL6-C16/1, C16, 1P, 6кА" @change="update" outlined></v-text-field>
                                        </v-col>
                                        <v-col cols="6">
                                            <v-text-field label="Наименование, тип электроприемника"  hint="Наименование, тип, обозначение чертежа принципиальной схемы" v-model="issue.desc" @change="update" outlined></v-text-field>
                                        </v-col>
                                    </v-row>
                                    <v-row align="center" class="mx-0">
                                        <v-col cols="4">
                                            <v-text-field label="Кабель обозначение"  v-model="issue.cable.label" hint="Примеры: QS, EATON IS-32/2, 32 A, 2P или DQ1, EATON PF6-40/2/003, AC, 30 мА, 1P+N, 40 A или 1QF3, EATON PL6-C16/1, C16, 1P, 6кА" @change="update" outlined></v-text-field>
                                        </v-col>
                                        <v-col cols="4">
                                            <v-text-field label="Кабель марка"  hint="Наименование, тип, обозначение чертежа принципиальной схемы" v-model="issue.cable.model" @change="update" outlined></v-text-field>
                                        </v-col>
                                        <v-col cols="4">
                                            <v-text-field label="Кабель длина, м"  hint="Наименование, тип, обозначение чертежа принципиальной схемы" v-model="issue.cable.length" @change="update" outlined></v-text-field>
                                        </v-col>
                                    </v-row>  
                                     <v-row align="center" class="mx-0">
                                        <v-col cols="6">
                                            <v-text-field label="Труба обозначение"  v-model="issue.pipe.label" hint="Примеры: QS, EATON IS-32/2, 32 A, 2P или DQ1, EATON PF6-40/2/003, AC, 30 мА, 1P+N, 40 A или 1QF3, EATON PL6-C16/1, C16, 1P, 6кА" @change="update" outlined></v-text-field>
                                        </v-col>
                                        <v-col cols="6">
                                            <v-text-field label="Труба длина, м"  hint="Наименование, тип, обозначение чертежа принципиальной схемы" v-model="issue.pipe.length" @change="update" outlined></v-text-field>
                                        </v-col>
                                    </v-row>
                                    <v-row align="center" class="mx-0">
                                        <v-col cols="2">
                                            <v-text-field label="Обозначение"  v-model="issue.label" hint="" @change="update" outlined></v-text-field>
                                        </v-col>
                                        <v-col cols="2">
                                            <v-text-field label="Руст., или Рном, кВ"  hint="" v-model="issue.load.installed.capacity" @change="update" outlined></v-text-field>
                                        </v-col>
                                        <v-col cols="2">
                                            <v-text-field label="Фаза А: Iуст, или Iном, А"  v-model="issue.load.installed.current_a" hint="" @change="update" outlined></v-text-field>
                                        </v-col>
                                        <v-col cols="2">
                                            <v-text-field label="Фаза B: Iуст, или Iном, А"  hint="" v-model="issue.load.installed.current_b" @change="update" outlined></v-text-field>
                                        </v-col>
                                        <v-col cols="2">
                                            <v-text-field label="Фаза C: Iуст, или Iном, А"  v-model="issue.load.installed.current_c" hint="" @change="update" outlined></v-text-field>
                                        </v-col>
                                        <v-col cols="2">
                                            <v-text-field label="cos φ"  hint="" v-model="issue.load.installed.power_factor" @change="update" outlined></v-text-field>
                                        </v-col>
                                    </v-row>
                                </v-card-text>
                            </v-slide-y-transition>                
                        </v-card>
                    </v-col>
                </transition-group>
            </draggable>
        </v-row>
      
        <v-row>
            <v-col cols="12">
                <v-card>
                    <v-system-bar  color="primary" @dblclick="newapp=!newapp"  window dark>
                        <v-icon>mdi-electric-switch</v-icon>
                        <span>Добавить электроприемник</span>
                        <v-spacer></v-spacer>
                        
                        <v-icon v-if="newapp" @click="newapp=!newapp">mdi-minus</v-icon>
                        <v-icon v-else @click="newapp=!newapp">mdi-checkbox-blank-outline</v-icon>
                    </v-system-bar>

                    <v-slide-y-transition>
                        <v-card-text v-if="newapp">   
                            <v-row align="center" class="mx-0">
                                <v-col cols="6">
                                    <v-text-field label="Аппарат отходящей линии"  v-model="app.device" hint="Примеры: QS, EATON IS-32/2, 32 A, 2P или DQ1, EATON PF6-40/2/003, AC, 30 мА, 1P+N, 40 A или 1QF3, EATON PL6-C16/1, C16, 1P, 6кА" outlined></v-text-field>
                                </v-col>
                                <v-col cols="6">
                                    <v-text-field label="Наименование, тип электроприемника"  hint="Наименование, тип, обозначение чертежа принципиальной схемы" v-model="app.desc" outlined></v-text-field>
                                </v-col>
                            </v-row>
                            
                            <v-row align="center" class="mx-0">
                                <v-col cols="4">
                                    <v-text-field label="Кабель обозначение"  v-model="app.cable.label" hint="Примеры: QS, EATON IS-32/2, 32 A, 2P или DQ1, EATON PF6-40/2/003, AC, 30 мА, 1P+N, 40 A или 1QF3, EATON PL6-C16/1, C16, 1P, 6кА" outlined></v-text-field>
                                </v-col>
                                <v-col cols="4">
                                    <v-text-field label="Кабель марка"  hint="Наименование, тип, обозначение чертежа принципиальной схемы" v-model="app.cable.model" outlined></v-text-field>
                                </v-col>
                                <v-col cols="4">
                                    <v-text-field label="Кабель длина, м"  hint="Наименование, тип, обозначение чертежа принципиальной схемы" v-model="app.cable.length" outlined></v-text-field>
                                </v-col>
                            </v-row>  
                            
                            <v-row align="center" class="mx-0">
                                <v-col cols="6">
                                    <v-text-field label="Труба обозначение"  v-model="app.pipe.label" hint="Примеры: QS, EATON IS-32/2, 32 A, 2P или DQ1, EATON PF6-40/2/003, AC, 30 мА, 1P+N, 40 A или 1QF3, EATON PL6-C16/1, C16, 1P, 6кА" outlined></v-text-field>
                                </v-col>
                                <v-col cols="6">
                                    <v-text-field label="Труба длина, м"  hint="Наименование, тип, обозначение чертежа принципиальной схемы" v-model="app.pipe.length" outlined></v-text-field>
                                </v-col>
                            </v-row>
                            
                            <v-row align="center" class="mx-0">
                                <v-col cols="2">
                                    <v-text-field label="Обозначение"  v-model="app.label" hint="" outlined></v-text-field>
                                </v-col>
                                <v-col cols="2">
                                    <v-text-field label="Руст., или Рном, кВ"  hint="" v-model="app.load.installed.capacity" outlined></v-text-field>
                                </v-col>
                                <v-col cols="2">
                                    <v-text-field label="Фаза А: Iуст, или Iном, А"  v-model="app.load.installed.current_a" hint="" outlined></v-text-field>
                                </v-col>
                                <v-col cols="2">
                                    <v-text-field label="Фаза А: Iуст, или Iном, А"  hint="" v-model="app.load.installed.current_b" outlined></v-text-field>
                                </v-col>
                                <v-col cols="2">
                                    <v-text-field label="Фаза А: Iуст, или Iном, А"  v-model="app.load.installed.current_c" hint="" outlined></v-text-field>
                                </v-col>
                                <v-col cols="2">
                                    <v-text-field label="cos φ"  hint="" v-model="app.load.installed.power_factor" outlined></v-text-field>
                                </v-col>
                            </v-row>
                            
                            <v-row>
                                <v-col>
                                    <v-btn dark small color="primary" @click="createapp">
                                        <v-icon dark>mdi-plus</v-icon> Добавить
                                    </v-btn>
                                </v-col>
                            </v-row>
                        </v-card-text>
                    </v-slide-y-transition> 
                </v-card>
            </v-col>
        </v-row>
       
    </v-container>
    <message :code="code" v-if="code > 0"></message>
    
    <v-dialog v-model="delete_dialog" width="500">

        <v-card>
            <v-toolbar color="red darken-3" dark>
            <v-toolbar-title>Confirm</v-toolbar-title>
            </v-toolbar>
    
            <v-card-text>
                <div class="pt-6">Удалить? Данное действие отменить будет невозможно.</div>
            </v-card-text>
            
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="red lighten-1" dense small @click="delete_diagram"> Да, удалить</v-btn>
                <v-btn color="primary" dense small @click="delete_dialog = false"> Отмена</v-btn>
            </v-card-actions>
        </v-card>		
    </v-dialog>
    
    <v-footer color="transparent">
        <v-col class="text-center caption" cols="12"><strong>Created:</strong> {{ body.created_at }} <strong>Last updated:</strong> {{ body.updated_at }} <strong>Endpoint:</strong> {{ endpoint }}</v-col>
  </v-footer>
  </div>
</template>


<script>
import axios from "axios";
import draggable from 'vuedraggable';
import message from "@/components/HttpMessage";
import qdelete from "@/components/Delete";

export default {
  name: "Diagram",

  components: {
    draggable,
    message,
    qdelete
  },
  props: {
    http_credentials: Object,
    endpoint: String,
    id: Number
  },
  data() {
    return {
      newapp: false,
      delete_dialog: false,
      expanded: [],
      body: {},
      app: {diagram: this.id, cable:{}, pipe:{}, load:{installed:{}}},
      menu: [
        { title: 'Скачать печатную форму', icon: 'mdi-file-pdf-box', action: 'getpdf'},
        { title: 'Удалить схему', icon: 'mdi-delete', action: 'delete' },
      ],
      code: 0
    }
  },
  mounted() {
    this.refresh()
    //console.log(this.body)
  },
  watch:{
    body: {
        handler: function(){
            //console.log(this.body) 
           
            },
        deep: true
    }
  },
  methods: {
      select(component){
			this.$emit('setWindow', component);
		},
        
    menuAction(action){
        switch(action){
            case 'getpdf':
                axios.get(this.endpoint, {responseType: 'blob', headers: {Authorization: 'Basic ' + btoa(this.http_credentials.username + ':' + this.http_credentials.password), Accept: 'application/pdf'}
                }).then((response) => {
                     var fileURL = window.URL.createObjectURL(new Blob([response.data]));
                     var fileLink = document.createElement('a');
   
                     fileLink.href = fileURL;
                     fileLink.setAttribute('download', 'phpSLDt-'+this.body.label+'('+this.id+').pdf');
                     document.body.appendChild(fileLink);
   
                     fileLink.click();
                });
                break;
            case 'delete':
                this.delete_dialog=true
                break;
            default:
                break;
        }
    },
    updateapp: function (){
        this.update()
        this.expanded=[]
    },
    createapp: function (){
        
      let request = JSON.parse(JSON.stringify(this.app));

      let endpoint = this.http_credentials.host + '/applications/';

      axios.post(endpoint, request, {headers: {Authorization: 'Basic ' + btoa(this.http_credentials.username + ':' + this.http_credentials.password), Accept:"*/*"}})
          .then((response) => {

            this.code = response.status
            this.refresh();
            this.app={diagram: this.id, cable:{}, pipe:{}, load:{installed:{}}}
          })
          .catch((error) => {
            this.code = error.response.status
          });

      this.code = 0
    },
    expand: function (item) {
        if(!this.expanded.includes(item)){
            this.expanded.push(item)
        } else {
            var index = this.expanded.indexOf(item);
            if (index !== -1) {
                this.expanded.splice(index, 1);
            }
        }
    },
    refresh: function () {
      this.body = []
      axios.get(this.endpoint, {headers: {Authorization: 'Basic ' + btoa(this.http_credentials.username + ':' + this.http_credentials.password), Accept: '*/*'}})
          .then(response => {
            this.body = response.data
          })
          .catch((error) => {
            this.code = error.response.status
          })
    },
    getPdf: function () {
      this.body = []
      axios.get(this.endpoint, {headers: {Authorization: 'Basic ' + btoa(this.http_credentials.username + ':' + this.http_credentials.password), Accept: 'application/pdf'}})
          .then(response => {
            this.body = response.data
          })
          .catch((error) => {
            this.code = error.response.status
          })
    },
    update() {

       let request = JSON.parse(JSON.stringify(this.body));

            let endpoint = this.http_credentials.host + '/diagrams/'+ this.id;

           axios.put(endpoint, request, {headers: {Authorization: 'Basic ' + btoa(this.http_credentials.username + ':' + this.http_credentials.password), Accept:"*/*"}})
          .then((response) => {

            this.code = response.status

          })
          .catch((error) => {
            this.code = error.response.status
          });

          this.code = 0

    },
    

    del(position) {
      let endpoint = this.http_credentials.host + '/applications/' + this.id+'-'+ position

      axios.delete(endpoint, {headers: {Authorization: 'Basic ' + btoa(this.http_credentials.username + ':' + this.http_credentials.password), Accept:"*/*"}})
          .then((response) => {
            this.code = response.status
                this.refresh()
          })
          .catch((error) => {
            if (error.response.status === 401) {
              //   this.unauthorized()
            }
            this.code = error.response.status
            //console.log("NOT UPDATED!")
          });

      this.code = 0
      //this.body = []

    },
    delete_diagram(){
        axios.delete(this.endpoint, {headers: {Authorization: 'Basic ' + btoa(this.http_credentials.username + ':' + this.http_credentials.password), Accept:"*/*"}})
            .then((response) => {
                this.code = response.status
                this.select('diagrams', 'diagrams')
            })
            .catch((error) => {
                this.code = error.response.status
            });
    }
    
  }

};
</script>
