<template>
    <div id="app">
  <v-app id="inspire">
  <footer class="v-footer v-sheet theme--light v-footer--fixed v-footer--padless" data-booted="true" style="left: 0px; right: 0px; bottom: 0px;">
      <div class="red lighten-1 text-center v-card v-card--flat v-sheet theme--light rounded-0" style="width: 100%;">
      <v-card
        class="flex"
        flat
        tile
      >
        <v-card-title class="teal">
          <strong class="subheading">¡Gracias por usar WhatsChat!   &nbsp;</strong><v-icon class="d-flex justify-center">mdi-chat-processing</v-icon>
  
          <v-spacer></v-spacer>
  
         
        </v-card-title>
  
        <v-card-text class="py-2 white--text text-center">
          {{ new Date().getFullYear() }} — <strong>Master en Diseño y Programación Web</strong>
        </v-card-text>
      </v-card>
      </div>
    </footer>
  </v-app>
</div>
</template>