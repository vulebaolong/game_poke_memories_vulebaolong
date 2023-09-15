<template>
    <div class="screen">
        <div :class="`container_card-${cardsContext.length}`">
            <card-flip
                v-for="(card, index) in cardsContext"
                :key="index"
                :imgBackFaceUrl="`images/${card}.png`"
                :card="{ index: index, value: card }"
                :ref="`card-${index}`"
                :length="cardsContext.length"
                :rules="rules"
                @onFlip="checkRule($event)"
            />
        </div>
    </div>
</template>

<script>
import CardFlip from "./CardRoot.vue";
export default {
    props: {
        cardsContext: {
            type: Array,
            default: function () {
                return [];
            },
        },
    },
    components: { CardFlip },
    data() {
        return {
            rules: [],
        };
    },
    methods: {
        checkRule(card) {
            if (this.rules.length === 2) return false;

            this.rules.push(card);

            // ĐÚNG
            if (
                this.rules.length === 2 &&
                this.rules[0].value === this.rules[1].value
            ) {
                this.$refs[`card-${this.rules[0].index}`][0].onStopFlipCard();
                this.$refs[`card-${this.rules[1].index}`][0].onStopFlipCard();
                this.rules = [];
                const disableEl = document.querySelectorAll(
                    ".screen .card.disable"
                );

                if (
                    disableEl &&
                    disableEl.length === this.cardsContext.length - 2
                ) {
                    setTimeout(() => {
                        this.$emit("onFinish");
                    }, 920);
                }
                return console.log("right");
            }

            // SAI
            if (
                this.rules.length === 2 &&
                this.rules[0].value !== this.rules[1].value
            ) {
                setTimeout(() => {
                    this.$refs[
                        `card-${this.rules[0].index}`
                    ][0].onFlipBackCard();
                    this.$refs[
                        `card-${this.rules[1].index}`
                    ][0].onFlipBackCard();
                    this.rules = [];
                }, 800);
                return console.log("wrong");
            }

            return false;
        },
    },
};
</script>

<style lang="css" scoped>
.screen {
    padding-top: 50px;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.container_card-16 {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
}
.container_card-36 {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
}
.container_card-64 {
    display: grid;
    grid-template-columns: repeat(8, 1fr);
}
.container_card-100 {
    display: grid;
    grid-template-columns: repeat(10, 1fr);
}
</style>
