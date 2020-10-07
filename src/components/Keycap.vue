<template>
    <p class='keycap' v-bind:class="{isPressed:pressed}">{{keycap.toUpperCase()}}</p>
</template>

<script>
export default {
    name: 'keycap',
    data() {
        return {
        }
    },
    props: ['keycap', 'pressed']
}
</script>

<style scoped>
    .keycap {
        font-family: 'Secular One', sans-serif;
        width: 50px;
        height: 50px;
        text-align: center;
        margin: 3px;
        background-color: white;
        justify-content: center;
        align-items: center;
        display: flex;
        border-radius: 3px;
        border: 1px solid #ddd;
        box-shadow: 0 0 10px #ddd;
    }
    .isPressed {
        background-color: #eee;
    }
</style>