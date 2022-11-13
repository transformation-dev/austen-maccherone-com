<script>
	import PhotoCarousel from '$lib/components/PhotoCarousel.svelte';
	
	let photos = [
		{ src: '/images/Art9.jpg', text: 'human' },
		{ src: '/images/Art10_.jpg', text: 'masks' },
		{ src: '/images/Art11.jpg', text: 'dark woods' },
		{ src: '/images/AArt12.jpg', text: 'clown'},
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
