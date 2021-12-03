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
    
    <div class="flex text-center h-full items-center">

        <div class="container grid grid-cols-2 w-1/3 mx-auto gap-y-5 pb-5">

            <div>
                <img alt="record" src="img/general/record.gif" class="mx-auto" style="width: 95px; height: 90px;">
            </div>
            
            <div class="text-2xl text-red-300 mt-10" style="text-shadow:black 0px 0px 10px;">Recording...</div>

            <div class="col-span-2">
                <button on:click={closeModal} class="bg-gray-400 hover:bg-gray-500" id="stop">Stop</button>
            </div>
            
        </div>

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
        background-color: rgba(0,0,0,0.7);
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
		width: 150px;
        height: 50px;
	}

    button:active {
  		transform: translateY(3px);
    }
</style>