<template>
<main-monitor v-if="statusMatch==='default'" @onStart="onHandleStart($event)"/>
<interact-screen v-if="statusMatch==='match'" :cardContext ="setting.cardContext" 
@onFinished = "onGetResult()"
/>
<result-screen 
v-if="statusMatch==='result'" :timer ="timer"
@onStartAgain="statusMatch='default'"
/>
<copyright-screen/>
</template>

<script>
import { shuffled} from "./utils/array"
import MainMonitor from "./components/MainMonitor.vue"
import InteractScreen from "./components/InteractScreen.vue"
import ResultScreen from "./components/ResultScreen.vue"
import CopyrightScreen from "./components/CopyrightScreen.vue"

export default {
  name: 'App',
  components: {
    MainMonitor,
    InteractScreen,
    ResultScreen,
    CopyrightScreen
  },
  data() {
    return {
      setting:{
      totalOfBlocks: 0,
      cardContext:[],
      startedAt:null
      },
      statusMatch: "default",
    }
  },
  
  methods: {
    onHandleStart(config){
      console.log('hey',config);
      this.setting.totalOfBlocks = config.totalOfBlocks;
      const firstCard = Array.from({length:this.setting.totalOfBlocks/2},(_,i)=>i+1) 
      const secondCard =[...firstCard];
      const cards = [...firstCard,...secondCard];
      this.setting.cardContext = shuffled(shuffled(shuffled(shuffled(cards))));
      this.setting.startedAt = new Date().getTime();
      console.log(this.setting.cardContext)
      this.statusMatch= "match";
    },
    onGetResult(){
      this.timer = new Date().getTime() - this.setting.startedAt;
      this.statusMatch= "result";
    }
  },
}
</script>

<style>

</style>
