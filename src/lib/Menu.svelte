<script>
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();

    export let isPlaying;
    export let rhythms;
    export let bpm;

    function toggle() {
        isPlaying.val = !isPlaying.val;
    }
</script>

<div class="menu-container">
    <div class="menu">

        <button on:click={toggle}>{#if isPlaying.val}⏸️{:else}▶️{/if}</button>
        <br />

        <p>BPM:</p>
        <input bind:value={bpm} type="number" step="5" />
        <br />

        <ul>
            {#each rhythms as rhythm}
                1 / <input bind:value={rhythm} type="number" />
            {/each}
        </ul>
    </div>
</div>


<style>
    .menu :global(ul) {
        padding-left: 20px;
    }

    .menu-container {
        display: flex;
        flex-direction: column;
        height: 100%;
        padding-left: 1em;
        padding-right: 1em;
    }

    .menu {
        flex: 1;
    }

    div {
        height: 100%;
    }
</style>