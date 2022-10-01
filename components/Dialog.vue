<template>
  <v-layout row justify-center >
    <v-dialog
      v-model="dialog"
      fullscreen
      hide-overlay
      transition="dialog-bottom-transition"
      @keydown.esc="$emit('close')"
    >
      <v-card color="rgba(0,0,0,0.9)" @mousedown.capture="handleKeys">
        <v-toolbar color="rgba(0,0,0,0.9)" fixed>
          <v-spacer></v-spacer>
          <v-toolbar-title class="headline">{{game.name}}</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
            <v-btn icon dark @click.stop="$emit('close')">
              <v-icon>close</v-icon>
            </v-btn>
          </v-toolbar-items>
        </v-toolbar>
        <v-card-text >
          <v-layout column justify-center>
            <v-layout row shrink justify-center>
              <v-flex xs4>
                <v-container text-xs-start justify-center>
                  <v-flex xs12>
                    <v-container class="mt-2">
                <v-img :src="'images/'+game.image"></v-img>
                </v-container>
                  </v-flex>
                </v-container>
              </v-flex>
              <v-flex xs8>
                <v-container text-xs-start justify-center>
                  <v-flex xs12 v-if="this.game.description">
                    <v-container class="mb-0 pb-0 mt-2">
                      <v-card >
                        <blockquote class="blockquote px-3">{{game.description}}</blockquote>
                      </v-card>
                    </v-container>
                  </v-flex>
                  <v-layout row justify-start shrink>
                    <v-flex xs12>
                      <v-container class="mt-2">
                        <v-card class="text-xs-left">
                          <blockquote class="blockquote font-weight-medium">Tipo:&nbsp;&nbsp;{{game.classification}}</blockquote>
                          <blockquote class="blockquote font-weight-medium">Eposodios:&nbsp;&nbsp;{{game.episodes}}</blockquote>
                          <blockquote class="blockquote font-weight-medium" style="padding-bottom:0">Géneros:</blockquote>
                          <v-layout row wrap class="pl-1 pr-1">
                            <blockquote class="blockquote" v-for="genre in game.categories" :key="genre"><v-icon>mdi-meteor</v-icon>{{genre}}</blockquote>
                          </v-layout>
                        </v-card>

                      </v-container>
                    </v-flex>
                  </v-layout>
                </v-container>
              </v-flex>
            </v-layout>
          </v-layout>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-layout>
</template>

<script>
export default {
  name: "Dialog",
  props: {
    dialog: Boolean,
    game: Object
  },
  data: () => ({}),
  computed: {},
  methods: {
    handleKeys: function(event) {
      if (event.button == 3){
        this.$emit('close')
      }
    },
  }
};
</script>