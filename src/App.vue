<script setup>
import { ref } from 'vue'

const idItem = ref(null)

const handleDragStart = (event, idItemDrag) => {
  idItem.value = idItemDrag

  const elementDrag = document.getElementById(idItem.value)
  const rect = elementDrag.getBoundingClientRect()

  const offsetX = rect.width / 2
  const offsetY = rect.height / 2
  event.dataTransfer.setData('text/plain', idItemDrag)
  event.dataTransfer.setDragImage(elementDrag, offsetX, offsetY)
}

const handleDragOver = (event) => {
  event.preventDefault()
}

const handleDrop = (event) => {
  const elementDrag = document.getElementById(idItem.value)
  const rect = event.currentTarget.getBoundingClientRect()

  let offsetX = event.clientX - rect.left - elementDrag.offsetWidth / 2
  let offsetY = event.clientY - rect.top - elementDrag.offsetHeight / 2

  offsetX = Math.max(0, Math.min(offsetX, rect.width - elementDrag.offsetWidth))
  offsetY = Math.max(0, Math.min(offsetY, rect.height - elementDrag.offsetHeight))

  elementDrag.style.top = offsetY + 'px'
  elementDrag.style.left = offsetX + 'px'
}
</script>

<template>
  <header>
    <h1>Arrastrar divs y redimensionar</h1>
  </header>

  <main>
    <div class="visor" @dragover="handleDragOver" @drop="handleDrop">
      <div
        class="draggable"
        id="draggableElement-1"
        :draggable="true"
        @dragstart="handleDragStart($event, 'draggableElement-1')"
        @dragover="handleDragOver"
      ></div>
      <div
        class="draggable two"
        id="draggableElement-2"
        :draggable="true"
        @dragstart="handleDragStart($event, 'draggableElement-2')"
        @dragover="handleDragOver"
      ></div>
    </div>
  </main>
</template>

<style scoped>
.visor {
  position: relative;
  width: 500px;
  height: 800px;
  border: 1px solid red;
  margin-left: 50px;
}
.draggable {
  top: 0px;
  width: 100px;
  height: 100px;
  background-color: red;
  position: absolute;
  cursor: move;
}
.draggable.two {
  left: 150px;
}
</style>
