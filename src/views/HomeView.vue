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
            <v-text-field label="Nueva tarea..." outlined v-model="task"></v-text-field>
          </v-col>
          <!-- BOTON PARA ENVIO DE FORMULARIO -->
          <v-col cols="10">
            <v-btn block class="primary" large :disabled="!okBtn" @click="addTask">Agregar <v-icon right dark>mdi-plus-thick</v-icon></v-btn>
          </v-col>
        </v-row>
      </v-col>
      <!-- LISTA DINAMICA -->
      <v-col cols="6">
        <v-card>
          <h2 class="text-center">LISTA DE TAREAS PENDIENTES</h2>
          <br>
          <div v-for="(row, index) in rows" :key="index">
            <ul>
              <li>
                {{ index + 1 }}. {{ row.task }}
              </li>
            </ul>
          </div>
        </v-card>
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
        rows: null
      }
    },
    // METODOS CALCULADOS
    computed:{
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
        axios.post('http://localhost:3000/addTask', {
          task: this.task
        })
        .then((results) => {
          if(results.data.status){  
            alert("Tarea añadida correctamente")
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
        axios.post('http://localhost:3000/getTasks')
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
      }
    }
  }
</script>
