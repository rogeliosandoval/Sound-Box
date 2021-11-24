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
        <article class="container mx-auto clip text-center pt-5">
            <p class="text-2xl font-bold pb-2">{soundClip.label}</p>
            <audio class="mx-auto w-1/3" src={soundClip.srcURL} controls="true"></audio>
            <div class="pt-3">
                <button id={soundClip.id} class="delete bg-red-400 hover:bg-red-500" type="button" on:click={deleteSoundClip(soundClip.id)}>Delete</button>
                <button type="button" class="edit bg-yellow-500 hover:bg-yellow-600" on:click={editSoundClip(soundClip.id)}>Edit Name</button>
            </div>
        </article>
    {/each}
{/if}

<style>

    .delete, .edit{
        border: 2px solid #000000;
		padding: 2px;
		width: 90px;
    }

    audio::-webkit-media-controls-panel{
		background: rgb(148, 197, 228);
	}

    button:active {
  		transform: translateY(3px);
	}

</style>