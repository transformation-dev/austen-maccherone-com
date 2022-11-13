<script>
	import PhotoCarousel from '$lib/components/PhotoCarousel.svelte';

	let photos = [
		{ src: '/images/AArt1.jpg', text: 'Flowers (ink)' },
		{ src: '/images/AArt2.jpg', text: 'Real Life Perspective (stained with coffee)' },
		{ src: '/images/A_Art3.jpg', text: 'Still Life (charcoal on grey canson paper)' },
		{ src: '/images/AArt4.jpg', text: 'Color Wheel (acryllic on bristol) '},
		{ src: '/images/AArt5.jpg', text: '"Fire and Water" (watercolor and acryllic)' },
		{ src: '/images/AArt6_.jpg', text: '"The Composer" (sharpie on bristol)' },
		{ src: '/images/AArt7.jpg', text: 'Lady With a Hat (charcoal on tan canson paper)' },
	];
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Austen Maccherone's website" />
</svelte:head>

<section>
	<PhotoCarousel {photos} />
</section>

<h1 id="description">Hello </h1>
<body> My passion for creation and innovation is what drives me to work and the results never fail to shock and amaze me. I love nature, it is something that is fabricated within all of us. There is a power in the intuition carved into the mind. My opinionated personality and exaggerated nature are what bring my ideas to life. 
</body>

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
