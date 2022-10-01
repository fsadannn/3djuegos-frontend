<template>
  <v-flex xs3>
    <v-layout column my-2 px-2 fill-height>
      <v-hover>

      <v-img
        slot-scope="{ hover }"
        :src="'images/'+game.image"
        :max-height="0.3*height"
        :max-width="0.2*width"
        gradient="transparent,transparent,black"
        :lazy-src="lazy"
      >
        <v-expand-transition>
          <div
           @click.stop="$emit('dialog',index)"
            v-if="hover"
            class="d-flex transition-fast-in-fast-out  v-card--reveal  white--text"
            style="height: 100%; background-color:rgba(0,0,0,0.7)"
          >
            <v-layout column  justify-end text-xs-center>

              <v-spacer></v-spacer>

              <v-layout justify-center shrink>
                <span class="px-3 py-1 purple darken-1" style="border-radius: 1rem;">{{game.classification}}</span>
              </v-layout>

              <v-spacer></v-spacer>

              <v-layout row justify-center >
                <v-flex xs12>
                  <v-btn class="mb-5" color="primary" @click.stop="$emit('dialog',index)" round>Detalles</v-btn>
                </v-flex>

              </v-layout>
            </v-layout>
          </div>
        </v-expand-transition>
        <v-layout column fill-height justify-end>
          <!-- <v-spacer></v-spacer> -->
          <v-layout row shrink>
            <v-flex xs10 class="pt-2 pl-2" >
            <span>{{game.name}}</span>
            </v-flex>
            <v-flex xs2>

            <!--<v-avatar  color="rgba(70,70,70,0.8)" >
              <span>{{game.aggregateRating.ratingValue== "\u00a0" ? '-': game.aggregateRating.ratingValue  }}</span>
            </v-avatar>-->
            </v-flex>
          </v-layout>
        </v-layout>
        <v-layout
          slot="placeholder"
          fill-height
          align-center
          justify-center
          ma-0
        >
          <v-progress-circular indeterminate color="grey lighten-5"></v-progress-circular>
        </v-layout>
      </v-img>
      </v-hover>
    </v-layout>
  </v-flex>
</template>

<style scoped>
div >>> .v-image__image--cover {
  background-size: 100% 100%;
}
</style>

<script>
import Dialog from "../components/Dialog"
export default {
  props: {
    game: Object,
    height: Number,
    width: Number,
    index: Number
  },
  data(){
    return{
      dialog:false,
      activeGame:null,
      lazy:require('../assets/img/lazy.jpg')
    }
  },
  components:{
  },
  computed:{},
  methods:{}
};
</script>
