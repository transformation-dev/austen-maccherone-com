<script>
	import Counter from './Counter.svelte';
	import welcome from '$lib/images/svelte-welcome.webp';
	import welcome_fallback from '$lib/images/svelte-welcome.png';
	import SliderGallery from '../components/SliderGallery.svelte';

	import magpie from '$lib/images/magpie.jpg';
	import flick from '$lib/images/flick.jpg';
	import blackbird from '$lib/images/blackbird.jpg';

	let imgArray = [
		{ src: magpie, text: "Magpie" },
		{ src: flick, text: "Flick" },
		{ src: blackbird, text: "Blackbird" },
	];

</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>

	<h1>
		Welcome
	</h1>

	<SliderGallery {imgArray} />


</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		display: block;
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	.welcome img {
		position: absolute;
		width: 100%;
		height: 100%;
		top: 0;
		display: block;
	}
</style>
