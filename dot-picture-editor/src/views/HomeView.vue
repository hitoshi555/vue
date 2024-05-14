<script setup lang="ts">
import { ref, onMounted, type Ref } from 'vue'

const canvas = ref<HTMLCanvasElement | null>(null)
const ctx = ref<CanvasRenderingContext2D | null>(null)
const preview = ref<HTMLCanvasElement | null>(null)
const pctx = ref<CanvasRenderingContext2D | null>(null)
const grid = ref({ x: 16, y: 16 })

const clear = (
  canvas: Ref<HTMLCanvasElement | null>,
  ctx: Ref<CanvasRenderingContext2D | null>
) => {
  if (ctx.value) {
    ctx.value.fillStyle = '#000000'
    ctx.value.fillRect(0, 0, canvas.value!.width, canvas.value!.height)
  }
}

const getPos = (e: MouseEvent) => {
  if (!canvas.value) {
    return { x: 0, y: 0 }
  }
  return {
    x: e.clientX - canvas.value.getBoundingClientRect().left,
    y: e.clientY - canvas.value.getBoundingClientRect().top
  }
}

const getGrid = (e: MouseEvent) => {
  if (!canvas.value) {
    return { x: 0, y: 0 }
  }
  const pos = getPos(e)
  return {
    x: Math.floor(pos.x / (canvas.value.width / grid.value.x)),
    y: Math.floor(pos.y / (canvas.value.height / grid.value.y))
  }
}

const mouseDown = (e: MouseEvent) => {
  if (!ctx.value || !canvas.value) {
    return undefined
  }
  if (!pctx.value || !preview.value) {
    return undefined
  }
  const posGrid = getGrid(e)
  if (e.button == 0) {
    ctx.value.fillStyle = '#FFFFFF'
    pctx.value.fillStyle = '#FFFFFF'
  } else {
    ctx.value.fillStyle = '#000000'
    pctx.value.fillStyle = '#000000'
  }
  ctx.value.fillRect(
    posGrid.x * (canvas.value.width / grid.value.x) + 1,
    posGrid.y * (canvas.value.height / grid.value.y) + 1,
    canvas.value.width / grid.value.x - 2,
    canvas.value.height / grid.value.y - 2
  )
  pctx.value.fillRect(posGrid.x, posGrid.y, 1, 1)
}

const drawGrid = () => {
  if (!ctx.value || !canvas.value) {
    return undefined
  }
  ctx.value.strokeStyle = '#404040'
  ctx.value.beginPath()
  for (let x = 0; x < grid.value.x; x++) {
    ctx.value.moveTo(x * (canvas.value.width / grid.value.x), 0)
    ctx.value.lineTo(x * (canvas.value.width / grid.value.x), canvas.value.height)
  }
  for (let y = 0; y < grid.value.y; y++) {
    ctx.value.moveTo(0, y * (canvas.value.height / grid.value.y))
    ctx.value.lineTo(canvas.value.width, y * (canvas.value.height / grid.value.y))
  }
  ctx.value.stroke()
}

const saveImage = () => {
  if (preview.value !== null) {
    const link = document.createElement('a')
    link.href = preview.value.toDataURL('image/png')
    link.download = 'dot.png'
    link.click()
  }
}

onMounted(() => {
  if (preview.value) {
    pctx.value = preview.value.getContext('2d')
  }
  if (canvas.value) {
    ctx.value = canvas.value.getContext('2d')

    canvas.value.addEventListener('mousedown', mouseDown)
    canvas.value.addEventListener(`contextmenu`, (e) => {
      e.preventDefault()
    })
  }

  clear(canvas, ctx)
  clear(preview, pctx)

  drawGrid()
})
</script>

<template>
  <main>
    <canvas ref="canvas" width="512" height="512"></canvas>
    <span>...</span>
    <canvas ref="preview" width="16" height="16"></canvas>
    <span>...</span>
    <button @click="saveImage">download</button>
  </main>
</template>
