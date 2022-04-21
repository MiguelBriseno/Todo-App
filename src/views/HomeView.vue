<template>
<!-- CONTENEDOR PRINCIPAL -->
  <v-container>
    <br><br>
    <!-- CONTENEDOR SECUNDARIO -->
    <v-row justify="center">
      <!-- FORMULARIO -->
      <v-col cols="6">
        <v-row justify="center">
          <!-- INPUT PARA AGREGAR LA TAREA -->
          <v-col cols="10">
            <v-text-field label="Nueva tarea..." outlined v-model="task" v-on:keyup.enter="addTask"></v-text-field>
          </v-col>
          <!-- BOTON PARA ENVIO DE FORMULARIO -->
          <v-col cols="10">
            <v-btn block class="primary" large :disabled="!okBtn" @click="addTask">Agregar <v-icon right dark>mdi-plus-thick</v-icon></v-btn>
          </v-col>
        </v-row>
      </v-col>
      <!-- LISTA DINAMICA -->
      <v-col cols="6">
        <!-- CARD QUE MUESTRA LA LISTA DE TAREAS -->
        <v-card>
          <h2 class="text-center">LISTA DE TAREAS PENDIENTES</h2>
          <br>
          <!-- LISTA DINAMICA QUE MUESTRA LA LISTA DE TAREAS -->
          <v-row justify="center" align="center">
            <v-col cols="10" v-for="(row, index) in rows" :key="index">
              {{index + 1}}. {{row.task}} - <b>{{row.taskStatus}}</b><br><br>
              <!-- BOTONES PARA BORRAR O ACTUALIZAR LA TAREA -->
              <v-btn class="error" @click="deleteTask(row.id)"><v-icon>mdi-close-thick</v-icon></v-btn>&nbsp;&nbsp;&nbsp;&nbsp;
              <v-btn class="success" @click="aproveTask(row.id)"><v-icon>mdi-check-bold</v-icon></v-btn>&nbsp;&nbsp;&nbsp;&nbsp;
              <v-btn class="primary" @click="showDialog(row)"><v-icon>mdi-update</v-icon></v-btn>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
      <!-- MODAL PARA ACTUALIZAR LA TAREA -->
      <v-col cols="12">
        <v-dialog v-model="dialog" width="500">
          <v-card>
            <v-card-title>Actualizar tarea</v-card-title>
            <v-spacer></v-spacer>
            <div v-if="onlyTask != null">
              <v-row justify="center">
                <v-col cols="10">
                  <v-text-field v-model="onlyTask.task" v-on:keyup.enter="updateTask(onlyTask)"></v-text-field>
                </v-col>
                <v-col cols="5">
                  <v-btn class="primary" block @click="updateTask(onlyTask)">Actualizar</v-btn>
                </v-col>
                <v-col cols="5">
                  <v-btn class="error" block @click="closeDialog">Cancelar</v-btn>
                </v-col>
              </v-row>
            </div><br>
          </v-card>
        </v-dialog>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios'
  export default {
    // HOOK DE CREACION
    created(){
      this.getTasks()
    },
    // VARIABLES
    data(){
      return{
        task: null,
        rows: null,
        dialog: false,
        onlyTask: null
      }
    },
    // METODOS CALCULADOS
    computed:{
      /************************************************** PETICIONES AXIOS ********************************************************/
      // FUNCION PARA HABILITAR EL BOTON DE AGREGAR COMENTARIO
      okBtn(){
        let tmp = true
        if(this.task == null){
          tmp = false
        }
        return tmp
      }
    },
    // FUNCIONES
    methods:{
      // FUNCION PARA AÑADIR UNA NUEVA TAREA A LA BASE DE DATOS
      addTask(){
        axios.post('http://localhost:3000/tasks/addTask', {
          task: this.task
        })
        .then((results) => {
          if(results.data.status){  
            alert("Tarea añadida correctamente")
            this.getTasks()
            this.task = null
          }
          else{
            alert(results.data.error)
          }
        })
        .catch((error) => {
          alert(error)
        })
      },
      // FUNCION PARA OBTENER TODAS LAS TAREAS AÑADIDAS A LA BASE DE DATOS
      getTasks(){
        axios.post('http://localhost:3000/tasks/getTasks')
        .then((results) => {
          if(results.data.status){
            this.rows = results.data.data
          }
          else{
            alert(results.data.error)
          }
        })
        .catch((error) => {
          alert(error)
        })
      },
      // FUNCION PARA BORRAR UNA TAREA
      deleteTask(id){
        axios.post('http://localhost:3000/tasks/deleteTask', {
          id: id
        })
        .then((results) => {
          if(results.data.status){
            alert("Tarea eliminada correctamente")
            this.getTasks()
          }
          else{
            alert(results.data.status)
          }
        })
        .catch((error) => {
          alert(error)
        })
      },
      // FUNCION PARA APROBAR LAS TAREAS
      aproveTask(id){
        axios.post('http://localhost:3000/tasks/aproveTask', {
          id: id
        })
        .then((results) => {
          if(results.data.status){
            alert("Tarea terminada !")
            this.getTasks()
          }
          else{
            alert(results.data.error)
          }
        })
      },
      // FUNCION PARA ACTUALIZAR LA TAREA
      updateTask(object){
        axios.post('http://localhost:3000/tasks/updateTask', {
          id: object.id,
          task: object.task
        })
        .then((results) => {
          if(results.data.status){
            alert("Tarea actualizada !")
            this.getTasks()
            this.closeDialog()
          }
          else{
            alert(results.data.error)
          }
        })
        .catch((error) => {
          alert(error)
        })
      },
      /************************************************ METODOS LOCALES *************************************************/
      // FUNCION PARA MOSTRAR EL MODAL PARA ACTUALIZAR TAREA
      showDialog(object){
        this.dialog = true
        this.onlyTask = object
      },
      closeDialog(){
        this.dialog = false
        this.onlyTask = null
      }
    }
  }
</script>
