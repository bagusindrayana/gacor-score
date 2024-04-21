<script>
    import "../app.css";
</script>

<main class="p-o m-0 w-full">
    <slot />

</main>

<style lang="postcss">
    :global(html) {
        background-color: theme(colors.black);
    }
</style>