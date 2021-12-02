<script>

    export let button

    function playMyAudio(mySound){
        let myAudio = new Audio(mySound);
        myAudio.play();
    }

    //let buttonClass = button.isClicked ? button.color.secondary : button?.color?.primary;

</script>

<button on:mousedown={playMyAudio(button.sound)} class="{button.isClicked ? "transition duration-100 ease-in-out transform translate-y-3 translate-x-3 scale-140" : ""}">
    <div class="{button.isClicked ? "bg-red-700 box" : ""} bg-red-300 cards flex flex-col justify-center gap-x-2 max-w-2xl text-center box">

    <div class="font-bold text-3xl pt-3">{button.name}</div>
    <img class="mx-auto p-7" alt="{button.image}" src="{button.image}">
    <div class="font-bold text-xl pb-3">{button.key}</div>


    </div>
</button>

<style>
    img {
        width: 250px;
        height: 240px;
    }

    .cards{
        border: 5px solid black;
        box-shadow: 5px 5px 5px #000000;
        transition: background-color 0.25s linear
    }
    .cards:active {
        box-shadow: none;
    }

    .cards:hover{
        background-color: rgb(168, 72, 72);
    }

    button:active {
  		transform: translate(10px, 10px);
	}
</style>