<script>
    import { createEventDispatcher } from 'svelte';
	
    const dispatch = createEventDispatcher();

    //becomes a prop
    export let soundClips;

    function deleteSoundClip(selectedId){
        dispatch('deleteSoundClip', selectedId);
	}

    function editSoundClip(id) {
        dispatch('editSoundClip', id);
    }
</script>

{#if soundClips.length > 0}
    {#each soundClips as soundClip}
        <article class="clip text-center pt-5">
            <p class="text-2xl font-bold pb-2">{soundClip.label}</p>
            <audio class="mx-auto w-1/3" src={soundClip.srcURL} controls="true"></audio>
            <button id={soundClip.id} type="button" on:click={deleteSoundClip(soundClip.id)}>Delete</button>
            <button type="button" on:click={editSoundClip(soundClip.id)}>Edit</button>
        </article>
    {/each}
{/if}

<style>

    audio::-webkit-media-controls-panel{
		background: lightyellow;
	}

</style>