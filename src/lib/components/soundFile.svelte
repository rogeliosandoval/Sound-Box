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
            <p class="pb-2">{soundClip.label}</p>
            <audio class="mx-auto w-1/3" src={soundClip.srcURL} controls="true"></audio>
            <div class="pt-3">
                <button id={soundClip.id} class="delete bg-red-400 hover:bg-red-500" type="button" on:click={deleteSoundClip(soundClip.id)}>Delete</button>
                <button type="button" class="edit bg-yellow-500 hover:bg-yellow-600" on:click={editSoundClip(soundClip.id)}>Edit Name</button>
            </div>
        </article>
    {/each}
{/if}

<style>

    p {
        color: white;
        font-family: sans-serif;
        font-size: 25px;
        font-weight: bold;
        text-shadow: 0.0px 10.0px 0.02px #000, 9.8px 2.1px 0.02px #000, 4.2px -9.1px 0.02px #000, -8.0px -6.0px 0.02px #000, -7.6px 6.5px 0.02px #000, 4.8px 8.8px 0.02px #000, 9.6px -2.8px 0.02px #000, -0.7px -10.0px 0.02px #000, -9.9px -1.5px 0.02px #000, -3.5px 9.4px 0.02px #000, 8.4px 5.4px 0.02px #000, 7.1px -7.0px 0.02px #000, -5.4px -8.4px 0.02px #000, -9.4px 3.5px 0.02px #000, 1.4px 9.9px 0.02px #000, 10.0px 0.8px 0.02px #000, 2.9px -9.6px 0.02px #000, -8.7px -4.8px 0.02px #000, -6.6px 7.5px 0.02px #000, 5.9px 8.0px 0.02px #000, 9.1px -4.1px 0.02px #000, -2.1px -9.8px 0.02px #000, -10.0px -0.1px 0.02px #000, -2.2px 9.8px 0.02px #000, 9.1px 4.2px 0.02px #000, 6.1px -8.0px 0.02px #000, -6.5px -7.6px 0.02px #000, -8.8px 4.7px 0.02px #000, 2.7px 9.6px 0.02px #000, 10.0px -0.6px 0.02px #000, 1.5px -9.9px 0.02px #000, -9.3px -3.6px 0.02px #000, -5.5px 8.4px 0.02px #000, 7.0px 7.2px 0.02px #000, 8.5px -5.3px 0.02px #000, -3.4px -9.4px 0.02px #000, -9.9px 1.3px 0.02px #000, -0.8px 10.0px 0.02px #000, 9.6px 2.9px 0.02px #000, 4.9px -8.7px 0.02px #000, -7.5px -6.7px 0.02px #000, -8.1px 5.9px 0.02px #000, 4.0px 9.2px 0.02px #000, 9.8px -2.0px 0.02px #000, 0.2px -10.0px 0.02px #000, -9.7px -2.3px 0.02px #000, -4.3px 9.0px 0.02px #000, 7.9px 6.1px 0.02px #000
    }

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