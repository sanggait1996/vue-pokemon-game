<template>
    <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"></main-screen>
    <interact-screen 
    v-if="statusMatch === 'ingame'"
    :cardsContext="settings.cardsContext"
    @onFinish="onGetResult"
    />
    <result-screen 
    v-if="statusMatch === 'finished'" 
    :timer="timer" 
    @onStartAgain="statusMatch = 'default'"
    />
    <copy-right />
</template>

<script>
import MainScreen from "./components/MainScreen.vue"
import InteractScreen from "./components/InteractScreen.vue"
import ResultScreen from "./components/ResultScreen.vue";
import CopyRight from "./components/CopyRight.vue";

import { shuffled } from "./ultils/array"

export default {
    name: "App",
    components: {
        MainScreen,
        InteractScreen,
        ResultScreen,
        CopyRight,
    },
    data() {
        return {
            settings: {
                totalBlocks: 0,
                cardsContext: [],
                startedAt: null,
            },
            statusMatch: "default",
            timer: 0,
        }
    },
    methods: {
        onHandleBeforeStart(config) {
            this.settings.totalBlocks = config.totalBlocks
            const firstCards = Array.from({ length: this.settings.totalBlocks / 2}, 
            (_, i) => i + 1
            )
            const secondCards = [...firstCards]
            const cards = [...firstCards, ...secondCards]
            this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))))
            this.settings.startedAt = new Date().getTime();

            // data ready

            this.statusMatch = "ingame"
        },
        onGetResult() {
            // get timer
            this.timer = new Date().getTime() - this.settings.startedAt
            //switch to result 
            this.statusMatch = "finished"
        },
    }
};
</script>
