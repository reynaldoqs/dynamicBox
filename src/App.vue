<template>
<div id="app">
    <header class="w-100"></header>
    <div class="w-100 flex h-100">
        <nav class="gray-bg"></nav>
        <main class="h-100" scrollable>
            <div class="video-container w-100" >
                <drag-container
                openedPosition='bottom left'
                :openedH="200"
                :openedW="300"
                :sticky="true" :draggable="false" animate>
                    <div class="element"></div>
                </drag-container>
            </div>
            <div class="spam">
            </div>
        </main>
    </div>
</div>
</template>

<script>
import DragContainer from './components/DragContainer.vue'

export default {
    name: 'app',
    components: {
        DragContainer
    }
}
</script>

<style>
.spam{
  height: 1200px;
}
.element{
  width: 700px;
  height: 420px;
  background-image: url(./assets/video_demo.png);
  background-size: cover;
}
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    width: 100vw;
    height: 100vh;
    --h-header: 50px;
    --w-nav: 200px;
}

body {
    padding: 0;
    margin: 0;
}

*,
*::after,
*::before {
    box-sizing: border-box;
}

header {
    height: var(--h-header);
    border-bottom: 1px solid #d1d1d1;
    box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
}

nav {
    width: 200px;
}

main {
    width: calc(100% - var(--w-nav));
    overflow-y: auto;
}

.gray-bg {
    background-color: #e4e4e7
}

.white-bg {
    background-color: #f1f1f1
}

.w-100 {
    width: 100%;
}

.h-100 {
    height: calc(100% - var(--h-header));
}

.border-r {
    border-radius: 3px;
}

.flex {
    display: flex;
}

.video-container {
    background-color: #212121;
}

.scroller {
    overflow-y: scroll;
    scrollbar-color: #0A4C95 #C2D2E4;
}

.scroller::-webkit-scrollbar {
    width: 15px;
    height: 15px;
}

.scroller::-webkit-scrollbar-track-piece {
    background-color: #C2D2E4;
}

.scroller::-webkit-scrollbar-thumb:vertical {
    height: 30px;
    background-color: #0A4C95;
}
</style>
