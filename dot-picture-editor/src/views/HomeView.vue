<script setup lang="ts">
import {ref, onMounted} from 'vue'


    const canvas = ref<HTMLCanvasElement | null>(null);
    const ctx = ref<CanvasRenderingContext2D | null>(null);
    const grid = ref({x:16, y:16})

    const clear = () => {
      if (ctx.value) {
        ctx.value.fillStyle = "#000000";
        ctx.value.fillRect(0, 0, canvas.value!.width, canvas.value!.height);
      }
      drawGrid();
    };

    const getPos =(e: MouseEvent)=>{
      return {
        x: e.clientX - canvas.value.getBoundingClientRect().left,
        y: e.clientY - canvas.value.getBoundingClientRect().top,
      }
    };

    const getGrid =(e)=>{
      const pos = getPos(e)
      return {
        x: Math.floor(pos.x / (canvas.value.width/grid.value.x) ),
        y: Math.floor(pos.y / (canvas.value.height/grid.value.y) ),
      };
    };

    const mouseDown = (e: MouseEvent) =>{
      const posGrid = getGrid(e);
      if (e.button == 0){
        ctx.value.fillStyle = "#FFFFFF";
      }else{
        ctx.value.fillStyle = "#000000";
      }
      ctx.value.fillRect( posGrid.x * (canvas.value.width/grid.value.x) + 1,
      posGrid.y * (canvas.value.height/grid.value.y) + 1, 
        (canvas.value.width/grid.value.x) - 2,
        (canvas.value.height/grid.value.y) - 2 );
    };


    const drawGrid = ()=>{
      ctx.value.strokeStyle = "#404040";
      ctx.value.beginPath();
      for ( let x = 0; x < grid.value.x; x++ ){
        ctx.value.moveTo( x * (canvas.value.width/grid.value.x), 0);
        ctx.value.lineTo( x * (canvas.value.width/grid.value.x), canvas.value.height);
      }
      for ( let y = 0; y < grid.value.y; y++ ){
        ctx.value.moveTo( 0, y * (canvas.value.height/grid.value.y));
        ctx.value.lineTo( canvas.value.width, y * (canvas.value.height/grid.value.y));
      }
      ctx.value.stroke();
    };

    onMounted(() => {
      if (canvas.value) {
        ctx.value = canvas.value.getContext('2d');

        canvas.value.addEventListener( "mousedown", mouseDown );
        canvas.value.addEventListener(`contextmenu`, (e) => {
	    e.preventDefault();
    });
        clear();
      }
    });
</script>

<template>
  <main>
    <canvas ref="canvas" width="512" height="512"></canvas>
  </main>
</template>
