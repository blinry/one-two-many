<script>
    export let value;
    export let interactive = false;
    export let showValue = true;

    let height = Math.sqrt(value);
    let width = Math.sqrt(value);
    let currentlyResizing = false;

    function mousemove(event) {
        if (currentlyResizing) {
            width = event.clientX - event.target.getBoundingClientRect().left;
            height = event.clientY - event.target.getBoundingClientRect().top;
            value = width * height;
        }
    }
</script>

<div
    id="region"
    class={interactive ? "interactive" : ""}
    on:mousedown={() => {
        if (interactive) {
            currentlyResizing = true;
        }
    }}
    on:mouseup={() => {
        currentlyResizing = false;
    }}
    on:mousemove={mousemove}
>
    <div id="box" style="width: {width}px; height: {height}px;">
        {#if showValue}
            {Math.round(value)}
        {/if}
    </div>
</div>

<style>
    #region.interactive {
        width: 1000px;
        height: 1000px;
        background-color: #eee;
        user-select: none;
    }
    #box {
        border: 3px solid black;
        background-color: gray;
        box-sizing: border-box;
    }
    .interactive #box {
        cursor: nwse-resize;
        background-color: lightblue;
    }
</style>
