<script>
  import Sandbox from "./lib/Sandbox.svelte";
  import Menu from "./lib/Menu.svelte";

  let isPlaying = { val: true};
  let rhythms = [ 4, 5];
  let bpm = 90;

  function updateBpm() {
    bpm = bpm;
  }
</script>

<main>
  <audio id="boop">
    <source src="boop.mp3" type="audio/mp3"/>
  </audio>
  <div class="grid-container">
    <Menu {isPlaying} {rhythms} {bpm} />  
    <Sandbox {isPlaying} {rhythms} {bpm} on:updateBpm={updateBpm} />
  </div>
</main>

<style>
  .grid-container {
    display: grid;
    grid-template-columns: 1.1fr 6fr;
    height: 100%;
  }

  main {
    height: 100%
  }
</style>
