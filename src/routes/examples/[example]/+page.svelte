<script lang="ts">
    import { page } from '$app/state'

    const example = $derived(import(`../../../examples/${page.params.example}/index.svelte`))
</script>

{#await example then component}
    <component.default />
{/await}