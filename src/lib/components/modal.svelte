<!-- src/Modal.svelte -->
<script>
    import { createEventDispatcher } from 'svelte';
	
    const dispatch = createEventDispatcher();
	
    export let isOpenModal;
	
    function closeModal() {
        isOpenModal = false;
        dispatch('closeModal', { isOpenModal });
    }
</script>

<div id="background" style="--display: {isOpenModal ? 'block' : 'none'};"></div>
<div id="modal" style="--display: {isOpenModal ? 'block' : 'none'};">
    
    <div class="text-center">

        <div class="py-7">
            <img alt="record" src="img/record.gif" class="mx-auto" style="width: 100px; height: 100px;">
            <div class="text-2xl text-red-300" style="text-shadow:black 0px 0px 10px;">RECORDING...</div>
        </div>
    
        <button on:click={closeModal} class="bg-gray-400 hover:bg-gray-500" id="stop">Stop</button>
    </div>


</div>

<style>
    #background, #modal {
        animation-duration: 1s;
        animation-name: fade;
    }

    @keyframes fade{
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
    }

    #background {
        display: var(--display);
        background-color: rgba(0,0,0,0.3);
        position: fixed;
        z-index: 1;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
    }

    #modal {
        width: 600px;
        height: 500px;
        display: var(--display);
       	position: fixed;
        z-index: 2;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }

    #stop {
		border: 2px solid #000000;
		padding: 2px;
		width: 85px;
	}

    button:active {
  		transform: translateY(3px);
    }
</style>