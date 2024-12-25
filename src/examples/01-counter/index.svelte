<script lang="ts">
    let counter : number = $state(0);
</script>

<div class="container space">
    <h1>Counter</h1>
    <input type="text" bind:value={counter} readonly>
    
    <div class="button-group">
        <button onclick={() => counter++}>Count</button>
        <button class="outline secondary " onclick={() => counter = 0}>Reset</button>
    </div>
    
</div>

<style>
    .button-group {
        display: flex;
        justify-content: space-between;
    }
</style>