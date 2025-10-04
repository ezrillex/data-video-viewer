<script lang="ts">
import { onMount } from "svelte";
let configured = false;
let datalink = '';
let videos: {title: string, media: string}[] = []
let videoActual: {title: string, media: string} | undefined;

onMount(() => {
    datalink = localStorage.getItem('datalink') ?? ''
})
    async function decrypt(){
        const data = await (await fetch(datalink)).text()
        console.log(data)
        videos = JSON.parse(data)
        configured = true;
        localStorage.setItem('datalink', datalink)
    }

    function view(datos){
        console.log(datos)
        videoActual = datos;
    }


</script>

<h1>Video en linea.</h1>

{#if configured}
    {#if videoActual}
        <div class="ratio ratio-16x9">
        <video controls autoplay src="{videoActual.media}"></video>
        </div>
    {/if}

    {#each videos as video}
        <li class="mb-3">
            {video.title} <button on:click={()=>view(video)}>Ver Video</button>
        </li>
    {/each}
{:else}
    <label for="inputpass">url datos:</label>
    <input bind:value={datalink} id="inputpass" type="text">
    <button on:click={decrypt}>acceder</button>
{/if}