<template>
<div @mousedown.stop @click="$emit('iconClick')" class="icon-button">
    <img :src="require(`../assets/${this.icon}.svg`)" alt="icon">
</div>
</template>

<script>
export default {
    name: 'icon-button',
    props: {
        icon: String
    }
}
</script>

<style>
.icon-button {
    width: 30px;
    height: 30px;
    cursor: pointer;
    padding: 7px;
    padding-top: 6px;
    border-radius: 5px;
}

.icon-button:hover {
    background-color: rgba(200, 200, 200, .1)
}

.icon {
    width: 100%;
    height: 100%;
}
</style>
