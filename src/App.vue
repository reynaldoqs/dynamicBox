<template>
<div id="app">
  <div class="space"></div>
    <div class="left" scrollable>
      <div class="space"></div>
        <drag-container openedPosition='bottom left'
        :sticky="false" draggable animate>
            <div class="element">
                Element outside
            </div>
        </drag-container>
        <div class="spam">
          amanda
        </div>
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
.element {
    width: 500px;
    height: 200px;
    background-color: rgba(0, 0, 0, .7);
    color: white;
}
.space{
  height: 100px;
  background-color: blueviolet
}
.left {
  height: 80vh;
  overflow-y: auto;
  width: 80%;
}

.spam {
    height: 800px;
    background-color: aqua;
}

body {
    padding: 0;
    margin: 0;
}

* {
    box-sizing: border-box;
}

#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}
</style>
