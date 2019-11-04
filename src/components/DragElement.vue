<template>
<div class="drag-element" :style="{cursor:isOpened && !isExpanded?'all-scroll':'default'}">
    <div class="top-options aud-hidden">
        <div v-show="isOpened">
            <icon-button v-show="isExpanded" icon='compress' @iconClick="$emit('compressBox')"/>
            <icon-button v-show="!isExpanded" icon='expand' @iconClick="$emit('expandBox')" />
            <icon-button icon='close' @iconClick="$emit('closeBox')" />
        </div>
        <div v-show="!isOpened">
            <div>
                
            </div>
            <icon-button icon='clone' @iconClick="$emit('openBox')" />
        </div>
    </div>
    <slot name="element" />
    <div @mousedown.stop class="bottom-options aud-hidden">
        <slot name="bottom-options"/>
    </div>
</div>
</template>

<script>
import IconButton from './IconButton'
export default {
    data() {
        return {
            styleObject: {
                backgroundColor: 'red',
                width: null,
                height: null
            }
        }
    },
    props: {
        isOpened: Boolean,
        isExpanded: Boolean
    },
    components: {
        IconButton
    }
}
</script>

<style scoped>
.drag-element {
    height: 100%;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
}

.aud-hidden {
    opacity: 0;
    transition: .3s opacity ease;
}

.drag-element:hover .aud-hidden {
    opacity: 1;
}

.top-options,
.bottom-options {
    position: absolute;
    width: 100%;
    height: 42px;
    left: 0;
    padding: 6px;
}

.top-options {
    top: 0;
    background-color: rgba(0, 0, 0, .3);
}

.top-options > div {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.bottom-options {
    bottom: 0;
    background-color: rgba(0, 0, 0, .1);
    cursor: default;
}
</style>
