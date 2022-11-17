<script>
  import { onMount } from "svelte";

  export let bpm;
  export let rhythms;
  export let isPlaying;

  let canvas;
  let prevTime = new Date().getTime();
  let boop = new Audio('boop.mp3');
  let accumulations = rhythms.map(rhythm => 0);

  onMount(() => {
    if (!canvas.getContext) {
      console.log("Could not find canvas rendering context!");
      return;
    }

    requestAnimationFrame(draw);
  });

  function drawRhythms() {
    const date = new Date();
    let ctx = canvas.getContext("2d");

    ctx.canvas.width = window.innerWidth;
    ctx.canvas.height = window.innerHeight;

    for (let i = 0; i < rhythms.length; i++) {
      let radius = (i + 1) * 50;

      ctx.beginPath();
      ctx.arc(ctx.canvas.width / 2, ctx.canvas.height / 2, radius, 0, 2 * Math.PI);
      ctx.stroke();

      for (let j = 0; j < rhythms[i]; j++) {
        let theta = (bpm * date.getTime() / (1000 * 60)) +  j * ((2 * Math.PI) / rhythms[i]);
        console.log(bpm);
        //let theta = (date.getTime() / (1000 * 60) + (j * ((2 * Math.PI) / rhythms[i])));
        let x = radius * Math.cos(theta);
        let y = radius * Math.sin(theta);
        let center = coord(ctx, x, y);

        ctx.beginPath();
        ctx.arc(center[0], center[1], 10, 0, 2 * Math.PI);
        ctx.stroke();
      }
    }
  }

  function draw() {
    const date = new Date();

    if (isPlaying.val) {
      let ctx = canvas.getContext("2d");

      ctx.canvas.width = window.innerWidth;
      ctx.canvas.height = window.innerHeight;
    }

    drawRhythms();
    
    for (let i = 0; i < accumulations.length; i++) {
      if (accumulations[i] > (rhythms[i] * 1000 / bpm)) {
        document.getElementById("boop").play();
        accumulations[i] = 0;
      }
      accumulations[i] += date.getTime() - prevTime;
     }
     prevTime = date.getTime();

    requestAnimationFrame(draw);
  }

  function coord(ctx, x, y) {
    return [ctx.canvas.width / 2 + x,
            ctx.canvas.height / 2 + y];
  }
</script>

<div>
  <canvas bind:this={canvas} />
</div>

<style>
  div {
    position: relative;
    overflow: hidden;
    height: 100%;
  }

  canvas {
    width: 100%;
    margin: auto;
    display: block;
    position: absolute;
    left: 0;
    top: 0;
  }

  #lines {
    z-index: 0;
  }
</style>
