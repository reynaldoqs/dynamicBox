<template>
<div @mousedown.stop @click="$emit('iconClick')" class="icon-button">
    <component :is="icon"></component>
</div>
</template>

<script>
import clone from '../assets/clone'
import close from '../assets/close'
import compress from '../assets/compress'
import expand from '../assets/expand'
import window_close from '../assets/window_close'
export default {
    name: 'icon-button',
    props: {
        icon: String
    },
    components:{
        clone,
        close,
        compress,
        expand,
        window_close
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
