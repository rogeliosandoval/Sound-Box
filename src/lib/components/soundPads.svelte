<script>

    import { onMount } from "svelte";

	import Card from "$lib/components/card.svelte"

	let buttons = [
		{
			name: "Train",
			image: "img/train.png",
			sound: "audio/train.wav",
            key: "a",
            isClicked: false,
			color: {
				primary: "bg-red-300",
				secondary: "bg-red-600"
			}
		},
		{
			name: "Golfer",
			image: "img/golf.png",
			sound: "audio/golf.wav",
            key: "s",
            isClicked: false,
			color: {
				primary: "bg-red-300",
				secondary: "bg-red-600"
			}
		},
		{
			name: "Bell",
			image: "img/bell.png",
			sound: "audio/bell.wav",
            key: "d",
            isClicked: false,
			color: {
				primary: "bg-red-300",
				secondary: "bg-red-600"
			}
		},
		{
			name: "Gun",
			image: "img/gun.png",
			sound: "audio/gun.wav",
            key: "z",
            isClicked: false,
			color: {
				primary: "bg-red-300",
				secondary: "bg-red-600"
			}
		},
		{
			name: "Toilet",
			image: "img/toilet.png",
			sound: "audio/toilet.wav",
            key: "x",
            isClicked: false,
			color: {
				primary: "bg-red-300",
				secondary: "bg-red-600"
			}
		},
		{
			name: "Cash",
			image: "img/cash.png",
			sound: "audio/cash.wav",
			key: "c",
			isClicked: false,
			color: {
				primary: "bg-red-300",
				secondary: "bg-red-600"
			}
		},
		{
			name: "Droplet",
			image: "img/droplet.png",
			sound: "audio/droplet.wav",
			key: "q",
			isClicked: false,
			color: {
				primary: "bg-red-300",
				secondary: "bg-red-600"
			}
		},
		{
			name: "Laser",
			image: "img/laser.png",
			sound: "audio/laser.wav",
			key: "w",
			isClicked: false,
			color: {
				primary: "bg-red-300",
				secondary: "bg-red-600"
			}
		},
		{
			name: "Paper Tear",
			image: "img/paper.png",
			sound: "audio/paper.wav",
			key: "e",
			isClicked: false,
			color: {
				primary: "bg-red-300",
				secondary: "bg-red-600"
			}
		},
		{
			name: "Thunder",
			image: "img/thunder.png",
			sound: "audio/thunder.wav",
			key: "r",
			isClicked: false,
			color: {
				primary: "bg-red-300",
				secondary: "bg-red-600"
			}
		}
	]

	onMount(() => {
		document.addEventListener('keydown', (event) => {
			playMyAudio(event.key)
		})
		document.addEventListener('keyup', (event) => {
			setTimeout(() =>{
				buttons = buttons.map(btn => {
					btn.isClicked = false;
					return btn;
				})
				stopMyAudio(event.key)
			}, 200)
		})
	})

	function playMyAudio(keyCode){
        const button = buttons.filter(btn => btn.key == keyCode).shift();
		if(button){
			let myAudio = new Audio(button.sound);
			myAudio.play();
		}
		buttons = buttons.map(btn => {
			if(btn.key === keyCode) btn.isClicked = true;
			return btn;
		})
    }

    function stopMyAudio(keyCode){
		const button = buttons.filter(btn => btn.key == keyCode).shift();
		if(button){
			let myAudio = new Audio(button.sound);
			myAudio.pause();
		}
	}

</script>

<section class="font-sans">

	<section class="mt-10 flex flex-wrap mx-auto w-3/4 gap-5 justify-center">
		{#each buttons as button}
			<Card button={button} />
		{/each}
	</section>
	
</section>