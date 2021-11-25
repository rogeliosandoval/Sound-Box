<script>

    export let button

    function playMyAudio(mySound){
        let myAudio = new Audio(mySound);
        myAudio.play();
    }

</script>

<button on:mousedown={playMyAudio(button.sound)} class="{button.isClicked ? "transition duration-100 ease-in-out transform translate-y-5 scale-140" : ""}">
    <div class="cards flex flex-col justify-center gap-x-2 max-w-2xl text-center">

    <img class="mx-auto p-7" alt="{button.image}" src="{button.image}">

    <div class="font-bold text-3xl pb-5">{button.name}</div>

    </div>
</button>

<style>
    img {
        width: 250px;
        height: 250px;
    }

    .cards{
        border: 5px solid #000000;
        background: lightgrey;
    }

    .cards:hover{
        background-color: darkgrey;
    }

    button:active {
  		transform: translateY(15px);
	}
</style>