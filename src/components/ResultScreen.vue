<template>
    <div class="screen">
        <h1>✨ Congratulations ✨</h1>
        <h3>{{ Math.round(timer / 1000) }} seconds</h3>
        <button @click="handleStartAgain">Start Again</button>
    </div>
</template>

<script>
export default {
    props: {
        timer: {
            type: Number,
            required: true,
        },
    },
    methods: {
        handleStartAgain() {
            this.$emit("onStartAgain");
        },
    },
};
</script>

<style lang="css" scoped>
.screen {
    width: 100%;
    height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
button {
    font: var(--font);
    background: transparent;
    box-shadow: none;
    border: 1px solid var(--light);
    color: var(--light);
    margin: 1rem;
    padding: 1rem 1.25rem;
    border-radius: 0.5rem;
    font-size: 1.25rem;
    cursor: pointer;
    transition: background 0.3s ease-in-out;
}

button:hover {
    background-color: var(--light);
    color: var(--dark);
}
</style>
