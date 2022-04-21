<template>
  <v-container>
    <br><br>
    <v-row justify="center">
      <v-col cols="6">
        <v-row justify="center">
          <v-col cols="10">
            <v-text-field label="Nueva tarea..." outlined></v-text-field>
          </v-col>
          <v-col cols="10">
            <v-btn block class="primary" large>Agregar <v-icon right dark>mdi-plus-thick</v-icon></v-btn>
          </v-col>
        </v-row>
      </v-col>
      <v-col cols="6">
        <v-card>
          <h2 class="text-center">LISTA DE TAREAS PENDIENTES</h2>
          <br>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    
  }
</script>
