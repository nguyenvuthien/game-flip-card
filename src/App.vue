<template>
    <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"/>
    <interact-screen 
    v-if="statusMatch === 'match'" 
    :cardsContext = "settings.cardsContext"
    @onFinish = "onGetResult"
    />
    <result-screen 
        v-if="statusMatch === 'result'" 
        :timer="timer"
        @onStartAgain = "statusMatch = 'default'"
    />
    <copy-right/>
</template>
<script>
import MainScreen from './components/MainScreen.vue';
import InteractScreen from './components/InteractScreen.vue';
import ResultScreen from './components/ResultScreen.vue';
import CopyRight from './components/CopyRight.vue';
import {shuffled} from './utils/array'; 

export default {
    name:"App",
    components:{
        MainScreen,
        InteractScreen,
        ResultScreen,
        CopyRight
    },
    data(){
        return {
            settings: {
                totalOfBlocks: 0,
                cardsContext: [],
                startedAt: null
            },
            statusMatch: "default"
        }
    },
    
    methods: {
        onHandleBeforeStart(config) {
            this.settings.totalOfBlocks = config.totalOfBlocks;

            //tao 2 mang voi do dai totalOfBlocks
            const firstCards = Array.from({length: this.settings.totalOfBlocks / 2}, (_, i) => i + 1);
            const secondCards = [...firstCards];

            //gop 2 mang
            const cards = [...firstCards, ...secondCards];
            //xao tron phan tu mang
            this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))));

            this.settings.startedAt = new Date().getTime();

            //data ready
            this.statusMatch = "match";
        },
        onGetResult(){
            //get time
            this.timer = new Date().getTime() - this.settings.startedAt;
            // chuyen sang resultscreen component
            this.statusMatch = "result";
        }
    }
};
</script>
