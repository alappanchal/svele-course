<script>
    import { slide,fade } from "svelte/transition";
</script>
<style>
    span{
        display: inline-block;
        margin: 0 0.25rem;
        border-radius: 3px;
        border: 1px solid #cf0056 ;
        background-color: #cf0056;
        color: #fff;
        padding: 0 0.5rem;
        font-family: 'Lato', sans-serif;
        font-size: 0.8rem;
    }
</style>
<span transition:fade>
    <slot />
</span>