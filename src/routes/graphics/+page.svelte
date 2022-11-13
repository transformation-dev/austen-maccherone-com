<script>
	import PhotoCarousel from '$lib/components/PhotoCarousel.svelte';

	import art9 from '$lib/images/Art9.jpg';
	import art10 from '$lib/images/Art10_.jpg';
	import art11 from '$lib/images/Art11.jpg';
	import art12 from '$lib/images/Art12_4.jpg';
	

	let photos = [
		{ src: art9, text: 'human' },
		{ src: art10, text: 'masks' },
		{ src: art11, text: 'dark woods' },
		{ src: art12, text: 'clown'},
	];
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Austen Maccherone's website" />
</svelte:head>

<section>
	<PhotoCarousel {photos} />
</section>

<h1 id="description">Hello</h1>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	#description {
		text-align: left;
	}
</style>
