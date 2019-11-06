<template>
<div class="drag-mask" :style="styleObject">
    <icon-button @iconClick="$emit('iconClick')" icon='window_close'/>
</div>
</template>

<script>
import iconButton from './IconButton'
export default {
    data() {
        return {
            styleObject: {
                width: null,
                height: null
            }
        }
    },
    components:{
        iconButton
    }
}
</script>

<style scoped>
.drag-mask {
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>
