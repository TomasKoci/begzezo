<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    export let cislo;

    let clicked = false;
    export let cklickedcounter;
    let hide = false;

    const piko = () => {hide = true;}
    const klik = () => {
        if (clicked) return;

        clicked = !clicked;
        dispatch('kam', { cislo, piko });
    };
    $: if (cklickedcounter == 0) clicked = false;
</script>

<div class="bg-pink-900 text-center w-40 h-32 m-auto {hide && "invisible"} transition-all duration-500 eksdy" style="transform: rotateY({!clicked ? "180" : "0"}deg);" on:click={klik}>
    {cislo}
</div>

<style>
    .eksdy::after {
        content: '';
        display: block;
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        z-index: -10;
        transform-style: preserve-3d;
        backface-visibility: hidden;
        transform: rotateY(180deg);

        @apply bg-pink-900
    }

    .eksdy {
        transform-style: preserve-3d;
    }
</style>