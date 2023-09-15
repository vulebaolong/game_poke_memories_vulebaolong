<template>
   <div class="container">
        <main-screen
            v-if="statusMatch === 'default'"
            @onStart="onHandleBeforeStart($event)"
        />
        <interact-screen
            v-if="statusMatch === 'match'"
            :cardsContext="setting.cardsContext"
            @onFinish="onHandleFinish"
        />
        <result-screen
            v-if="statusMatch === 'result'"
            :timer="timer"
            @onStartAgain="onHandleStartAgain"
        />
   </div>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffled } from "./helper/array";
import ResultScreen from "./components/ResultScreen.vue";

export default {
    name: "App",
    components: { MainScreen, InteractScreen, ResultScreen },
    data() {
        return {
            setting: {
                totalOfBlocks: 0,
                cardsContext: [],
                startedAt: null,
            },
            statusMatch: "default",
            timer: 0,
        };
    },
    methods: {
        onHandleBeforeStart(config) {
            this.setting.totalOfBlocks = config.totalOfBlocks;

            const firstCards = Array.from(
                {
                    length: this.setting.totalOfBlocks / 2,
                },
                (_, i) => i + 1
            );

            const secondCards = [...firstCards];

            const cards = [...firstCards, ...secondCards];

            this.setting.cardsContext = shuffled(
                shuffled(shuffled(shuffled(cards)))
            );

            this.setting.startedAt = new Date().getTime();

            this.statusMatch = "match";
        },
        onHandleFinish() {
            this.timer = new Date().getTime() - this.setting.startedAt;

            this.statusMatch = "result";
        },
        onHandleStartAgain() {
            this.statusMatch = "default";
        },
    },
};
</script>
