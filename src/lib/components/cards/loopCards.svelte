<script>

    import { onMount } from "svelte";

    export let button;
    let myAudio;
    let isActive = false;

    onMount(() => {
		document.addEventListener('keydown', (event) => {
            if(event.key === 'Escape'){
                resetLoops()
            }
		})
	})

    function playMyAudio(button){
        if(!button.isClicked){
            myAudio = new Audio(button.sound);
            myAudio.play();
            myAudio.loop = true;
            button.isClicked = true;
        } else {
            myAudio.pause();
            myAudio.currentTime = 0;
            button.isClicked = false;
        }
        isActive = !isActive
    }

    function resetLoops(){
        myAudio.pause();
        myAudio.currentTime = 0;
        button.isClicked = false;
        isActive = false;
    }

</script>

<button on:mousedown={playMyAudio(button)} class="{button.isClicked ? "transition duration-100 ease-in-out transform translate-y-3 translate-x-3 scale-140" : ""}">
    <div class="{button.isClicked ? "key box" : ""} cards flex flex-col justify-center gap-x-2 max-w-2xl text-center box" class:active={isActive}>

    <div class="font-bold text-2xl pt-3">{button.name}</div>
    <img class="mx-auto p-7" alt="{button.image}" src="{button.image}">
    <div class="font-bold text-xl pb-3">{button.key}</div>


    </div>
</button>

<style>
    .box{
        box-shadow: 5px 5px 5px #000000;
    }

    .key {
        filter: brightness(85%);
    }

    .active {
		box-shadow: none;
        transform: translate(10px, 10px);
        background: url("/img/general/yellowpaper.jpeg");
        background-size: cover;
        filter: brightness(85%);
	}

    img {
        width: 190px;
        height: 180px;
    }

    .cards{
        background: url("/img/general/yellowpaper.jpeg");
        background-size: cover;
        border: 5px solid darkblue;
        transition: background-color 0.25s linear
    }

    .cards:hover{
        background: url("/img/general/yellowpaper.jpeg");
        background-size: cover;
        filter: brightness(85%);
    }

</style>