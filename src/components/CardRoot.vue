<template>
    <div class="card" :class="[`card-${length}`, { disable: isDisable }]">
        <div
            class="card_inner"
            :class="{ 'is-flipped': isFlipped }"
            @click="onToggleFlipCard"
        >
            <div class="card_face card_face--front">
                <div class="card_content"></div>
            </div>
            <div class="card_face card_face--back">
                <div
                    class="card_content"
                    :style="{
                        backgroundImage: `url(${require('@/assets/' +
                            imgBackFaceUrl)})`,
                    }"
                ></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        imgBackFaceUrl: {
            type: String,
            required: true,
        },
        card: {
            type: [String, Number, Array, Object],
        },
        length: {
            type: Number,
        },
        rules: {
            type: [String, Number, Array, Object],
        },
    },
    data() {
        return {
            isDisable: false,
            isFlipped: false,
        };
    },
    methods: {
        onToggleFlipCard() {
            if (this.isDisable) return false;
            if (this.rules.length > 1) return false;
            this.isFlipped = !this.isFlipped;
            if (this.isFlipped) this.$emit("onFlip", this.card);
        },

        onFlipBackCard() {
            this.isFlipped = false;
        },

        onStopFlipCard() {
            this.isDisable = true;
        },
    },
};
</script>

<style lang="css" scoped>
.card {
    display: inline-block;
    margin-right: 1rem;
    margin-bottom: 1rem;
    aspect-ratio: 4/5;
}
.card-16 {
    width: 150px;
    perspective: 300px;
}
.card-36 {
    width: 90px;
    perspective: 180px;
}
.card-64 {
    width: 70px;
    perspective: 140px;
}
.card-100 {
    width: 55px;
    perspective: 110px;
}
.card_inner {
    width: 100%;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
}
.card.disable .card_inner {
    cursor: default;
}
.card_inner.is-flipped {
    transform: rotateY(-180deg);
}
.card_face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    overflow: hidden;
    border-radius: 1rem;
    padding: 1rem;
    box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}
.card_face--front .card_content {
    background: url("../assets//images//icon_back.png") no-repeat center center;
    height: 100%;
    width: 100%;
}
.card-16 .card_face--front .card_content {
    background-size: 40px;
}
.card-36 .card_face--front .card_content {
    background-size: 30px;
}
.card-64 .card_face--front .card_content {
    background-size: 30px;
}
.card-100 .card_face--front .card_content {
    background-size: 20px;
}

.card_face--back {
    background-color: var(--light);
    transform: rotateY(180deg);
}
.card_face--back .card_content {
    background-size: contain;
    background-position: center center;
    background-repeat: no-repeat;
    height: 100%;
    width: 100%;
}
</style>
