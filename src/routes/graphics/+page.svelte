<script>
	import PhotoCarousel from '$lib/components/PhotoCarousel.svelte';
	
	let photos = [
		{ src: '/images/Art9.jpg', text: 'Human' },
		{ src: '/images/Art10_.jpg', text: 'Masks' },
		{ src: '/images/Art11.jpg', text: 'Dark Woods' },
		{ src: '/images/AArt12.jpg', text: 'Clown'},
	];
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Austen Maccherone's website" />
</svelte:head>

<section>
	<PhotoCarousel {photos} />
</section>

<h1 id="description">Digital Graphics</h1>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	#description {
		text-align: center;
	}
</style>
