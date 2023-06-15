<script>
    import Box from "./Box.svelte"

    let showSolution = false
</script>

<Box value={1000} />
<Box value={1000} interactive={true} showValue={showSolution} />

<button on:click={() => (showSolution = true)}>Reveal</button>
