<script>
    import Box from "./Box.svelte";

    let showSolution = false;
</script>

<Box value={100} />
<Box value={100} interactive={true} showValue={showSolution} />

<button on:click={() => (showSolution = true)}>Reveal</button>
