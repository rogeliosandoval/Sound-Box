<script>

    import { onMount } from "svelte";

	import GameCards from "$lib/components/cards/gameCards.svelte";

    let games = [
        {
            name: "Banana Coin",
            image: "img/game/bananacoin.png",
            sound: "audio/game/bananacoin.wav",
            key: "u",
            isClicked: false
        },
        {
            name: "Banjo",
            image: "img/game/banjo.png",
            sound: "audio/game/banjo.wav",
            key: "i",
            isClicked: false
        },
        {
            name: "Coin",
            image: "img/game/coin.png",
            sound: "audio/game/coin.wav",
            key: "o",
            isClicked: false
        },
        {
            name: "Jiggy",
            image: "img/game/jiggy.png",
            sound: "audio/game/jiggy.wav",
            key: "p",
            isClicked: false
        },
        {
            name: "Mask Salesman",
            image: "img/game/maskguy.png",
            sound: "audio/game/maskguy.wav",
            key: "[",
            isClicked: false
        },
        {
            name: "Mushroom",
            image: "img/game/mushroom.png",
            sound: "audio/game/mushroom.wav",
            key: "j",
            isClicked: false
        },
        {
            name: "Navi",
            image: "img/game/navi.png",
            sound: "audio/game/navi.wav",
            key: "k",
            isClicked: false
        },
        {
            name: "Note",
            image: "img/game/note.png",
            sound: "audio/game/note.wav",
            key: "l",
            isClicked: false
        },
        {
            name: "Pacman",
            image: "img/game/pacman.png",
            sound: "audio/game/pacman.wav",
            key: ";",
            isClicked: false
        },
        {
            name: "PERFECT",
            image: "img/game/perfect.png",
            sound: "audio/game/perfect.wav",
            key: "'",
            isClicked: false
        }
    ]

	onMount(() => {
		document.addEventListener('keydown', (event) => {
			playMyAudio(event.key)
		})
		document.addEventListener('keyup', (event) => {
			setTimeout(() =>{
				games = games.map(btn => {
					btn.isClicked = false;
					return btn;
				})
				stopMyAudio(event.key)
			}, 75)
		})
	})

	function playMyAudio(keyCode){
        const button = games.filter(btn => btn.key == keyCode).shift();
		if(button){
			let myAudio = new Audio(button.sound);
			myAudio.play();
		}
		games = games.map(btn => {
			if(btn.key === keyCode) btn.isClicked = true;
			return btn;
		})
    }

    function stopMyAudio(keyCode){
		const button = games.filter(btn => btn.key == keyCode).shift();
		if(button){
			let myAudio = new Audio(button.sound);
			myAudio.pause();
		}
	}

</script>

<section class="relataive font-sans">

	<section class="mt-5 flex flex-wrap mx-auto w-3/4 gap-5 justify-center">
		{#each games as button}
			<GameCards button={button} />
		{/each}
	</section>
	
</section>