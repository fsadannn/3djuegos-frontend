
<template> 
  <v-layout row wrap v-if="showGames&&type" v-resize="onResize">
    
    <!-- <v-card flat> -->
    <ToolBar :filters="filters"/>
    <!-- </v-card> -->
    <!-- {{games[0]}} -->
    <Cover v-for="(game,index) in showGames" :key="game.name" :game="game" :height="height" :index="index" :width="width" @dialog="openDialog"/>
      <Dialog :dialog="dialog" :game="activeGame?activeGame:{}" @close="dialog=false" />
    
  </v-layout>
  <!-- <v-layout v-else-if="showGames&&!type">
    <v-btn fab absolute right large @click="change" fixed>
      <v-icon light v-html="type?'mdi-timeline-text':'mdi-timeline-text-outline'"></v-icon>
    </v-btn>
    <ToolBar :filters="filters"/>
    <TimeLine :showGames="showGames" @dialog="openDialog"/>
      <Dialog :dialog="dialog" :game="activeGame?activeGame:{}" @close="dialog=false" />
   

  </v-layout> -->

  <v-layout v-else column fill-height justify-center>
    <v-layout row shrink justify-center>
      <v-progress-circular size="80" indeterminate color="primary"></v-progress-circular>
    </v-layout>
  </v-layout>
</template>

<script>
import goTo from 'vuetify/lib/components/Vuetify/goTo'
//import TimeLine from "../components/TimeLine";
import Cover from "../components/Cover";
import BottomSheet from "../components/BottomSheet";
import ToolBar from "../components/ToolBar";
import { setTimeout } from 'timers';
import Dialog from "../components/Dialog"
export default {
  data() {
    return {
      type: true,
      // value: "",
      height: 0,
      width: 0,
      backedGames: [],
      count: 0,
      fixedCount: 60, // number of anime to see, we use fixedCount*3 to simulate real scroll
      scrollCount: 0,
      savedPositionCover:0,
      savedPositionTime:0,
      changing:false,
      dialog:false,
      activeGame:null
    };
  },
  computed: {
    showGames() {
      return this.$store.getters.getGamesToShow;
    },
    lower() {
      return this.$store.getters.getLower;
    },
    upper() {
      return this.$store.getters.getUpper;
    },
    filters() {
      return this.$store.getters.getFilters;
    }
  },

  watch: {
    
  },
  components: {
    Cover,
    BottomSheet,
    ToolBar,
    //TimeLine,
    Dialog
  },
  created() {
    //console.log("created");
    if (!this.$store.getters.gamesAvailable) {
      this.$store.dispatch("fetchGames");
    }
    if (!this.$store.state.filters) {
      this.$store.dispatch("fetchFilters");
    }
    const lower = 0;
    const upper = this.fixedCount*3;
    this.$store.dispatch("callSetLimits", { from: lower, to: upper });
  },
  mounted() {
    // this.fetch()
   this.onResize()
    this.scroll();
    // this.getFilters()
  },
  methods: {
    openDialog: function(index){
      this.activeGame = this.showGames[index]
      this.dialog = true
      //console.log(this.activeGame)
    },
    onResize: function (){
       this.height = window.innerHeight;
    this.width = window.innerWidth;
    },
    // change: function(){
    //   this.changing = true
    //   if (this.type){
    //     this.savedPositionCover = window.pageYOffset
    //     this.type = !this.type
    //     this.$vuetify.goTo(this.savedPositionTime)

    //   } else{
    //     this.savedPositionTime = window.pageYOffset
    //     this.type = !this.type
    //     this.$vuetify.goTo(this.savedPositionCover)
    //   }
    // setTimeout(()=>{this.changing = false},1000)
     
    // },
   
    scroll: function() {
      window.onscroll = () => {
        const boundCount = this.fixedCount*3;
        //console.log('scroll');
        let bottomOfWindow =
          Math.abs(document.documentElement.scrollTop + window.innerHeight - document.documentElement.offsetHeight)<1;
        /*console.log(document.documentElement.scrollTop);
        console.log(window.innerHeight);
        console.log(document.documentElement.scrollTop + window.innerHeight);
        console.log(document.documentElement.offsetHeight);
        console.log(bottomOfWindow);
        console.log(this.backedGames.length);
        console.log(this.changing);
        console.log(bottomOfWindow && this.backedGames.length === 0 && !this.changing);*/
        if (bottomOfWindow && this.backedGames.length === 0 && !this.changing) {
          //console.log('scroll down')
          const upper = this.upper + this.fixedCount;
          const lower = Math.max(0,upper-boundCount)
          this.$store.dispatch("testLimits", {
            from: lower,
            to: upper
          });
          if(this.$store.getters.getlengthOfNewLimits===0) {return;}
          this.$store.dispatch("callSetLimits", {
            from: lower,
            to: upper
          });
          //console.log(this.lower);
          //console.log(this.upper);
          window.scrollTo(0, 1.81*document.documentElement.offsetHeight/3);
          return;
        }

        if (document.documentElement.scrollTop < 0.5 && this.lower > 1 && !this.changing) {
          //console.log('scroll up')
          const upper = this.upper - this.fixedCount;
          const lower = Math.max(0, upper-boundCount);
          this.$store.dispatch("callSetLimits", {
            from: lower,
            to: upper
          });
          window.scrollTo(0, 0.01*document.documentElement.offsetHeight/6);
        }

        //console.log(this.lower);
        //console.log(this.upper);
      };
    },
    // filter: function() {
    //   console.log("filtering");
    //   if (this.value !== "") {
    //     this.backedGames = this.backedGames.concat(this.showGames);
    //     this.showGames = [];
    //     this.showGames = this.showGames.concat(
    //       this.games.filter(game =>
    //         game.name.toLowerCase().includes(this.value.toLowerCase())
    //       )
    //     );
    //     this.$forceUpdate();
    //     console.log(this.showGames);
    //   }
    // },
    // restore: function() {
    //   this.showGames = this.backedGames;
    //   this.backedGames = [];
    //   this.$forceUpdate();
    //   this.value = "";
    // },
    // hover: function() {
    //   this.$forceUpdate();

    //   console.log("hover");
    // }
  }
};
</script>


