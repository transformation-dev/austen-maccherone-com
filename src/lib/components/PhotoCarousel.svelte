<script>

    // @ts-nocheck

	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';
	import { onMount } from 'svelte';

	import VanillaSwipe from 'vanilla-swipe';

	/**
	 * @type {Photo[]}
	 * 
	 * @typedef {object} Photo
	 * @property {string} src - Accepts: 1) a relative path to an image, 2) an absolute path to an image, or 3) an image imported from a file
	 * @property {string} text
	 */
	export let photos = [];
	
	export let duration = 500;  // The duration of the animation in milliseconds
	export let easing = cubicOut;  // A Svelte-style easing function to use for the animation

	let photoCount = photos.length;
	if (photoCount < 1) {
		throw new Error('PhotoCarousel requires at least one photo');
	}

	const options = {duration, easing};
	
	let currentIndex = 0;
	let isHovered = false;

	// There are 3 visible slots in the carousel but you can have as many as you want in the photos array.
	// If you have less than 3 photos, the photos will repeat to fill up the 3 visible (and 2 invisible) slots.

	// There are 2 additional slots, far left and far right.
	// They are turned perpendicular to the screen and they have zero width so they are invisible.
	// They are just there so the animation for the 3 visible slots have a starting point and an ending point.

	// The 5 slots in the carousel are labeled: minus2 (-2), minus1 (-1), center (0), plus1 (+1), and plus2 (+2).
	// This allows the use of the new array method .at() which will take negative indexes.
	// So, for example, photos.at(-2) will return the second to last photo in the photos array.
	// This makes it circular like a playlist on permenant repeat.

	// They will slide in from the left or right depending on the spin direction of the carousel 
	// which depends on the direction of the swipe or click.

	// translateZ, rotateY, left, and width are used to position and rotate each photo in the carousel.
	// There is a complex interaction between the translateZ, rotateY, and the perspective depth when using CSS 3D perspective.
	// Without that, the far left and far right slots would have been rotated by 90 degrees to be perpendicular to the screen.
	// This page is what helped me learn how to do CSS 3D perspective: 
	// https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transforms/Using_CSS_transforms
	// The below values are the result of a lot of experimentation.

	const motionValues = [
		{translateZ: 0, rotateY: 0, left: 20, width: 60},  // center 
		{translateZ: -6, rotateY: 30, left: 80, width: 20},  // center + 1 = right
		{translateZ: -17, rotateY: 60, left: 100, width: 0},  // center + 2 = far right
		{translateZ: -17, rotateY: -60, left: 0, width: 0},  // center - 2 = far left
		{translateZ: -6, rotateY: -30, left: 0, width: 20},  // center - 1 = left
	]

	// This next section sets up the motion values after the carousel is rotated
	const motionValuesMinus1 = structuredClone(motionValues);
	const tempMinus1 = motionValuesMinus1.shift();
	motionValuesMinus1.push(tempMinus1);

	const motionValuesPlus1 = structuredClone(motionValues);
	const tempPlus1 = motionValuesPlus1.pop();
	motionValuesPlus1.unshift(tempPlus1);

	// This sets up the Svelte "tweens" which will animate the photos in the carousel
	const tweenedMotionValues = tweened(motionValues, options);

	function rotate(delta) {
		isHovered = false;  // Necessary when swiping is used or if/when keystroke support is added
		if (delta === -1 ) {
			tweenedMotionValues.set(motionValuesMinus1);
		} else if (delta === +1) {
			tweenedMotionValues.set(motionValuesPlus1);
		} else {
			throw new Error('delta must be -1 or +1');
		}
		// Once the above animation finishes, the following section resets everything to the starting position
		// and at the same instant shifts the index so everything is in the expected slot for the next rotate.
		// Sometimes there is a slight flicker for this step, but it is rare and not noticeable unless you are looking for it.
		// There was probably a better way to do this, but I couldn't figure it out.
		setTimeout(() => {
			tweenedMotionValues.set(motionValues, {duration: 0});
			currentIndex = currentIndex + delta;
		}, duration);
	}

	function swipe(evt, eventData) {
		if (eventData.directionX === "LEFT") {
			rotate(+1)
		} else if (eventData.directionX === "RIGHT") {
			rotate(-1)
		}
	}

	let carouselContainer;

	onMount(() => {
		const isTouchEventsSupported = VanillaSwipe.isTouchEventsSupported();

		const VS = new VanillaSwipe({
			element: carouselContainer,
			onSwiping: null,
			onSwiped: swipe,
			mouseTrackingEnabled: true,
		});

		VS.init();
	});

</script>

<div style="height:50px;">
	<div id="photo-text" class:hidden={!isHovered} class="centered">{photos.at((currentIndex) % photoCount).text}</div>
</div>

<div bind:this={carouselContainer} class="carouselContainer">
	
	<!-- 
	Since the below divs are positioned absolutely, their order does not determine where on the screen
	they will appear but it does determine which ones are on top of the others during the animation.
	So, first come the far left and far right divs that are essentially invisible except
	when the carousel rotates. By placing them first, they are behind the other divs.
	Then come the divs immediately to left and right of center.
	Finally, the center comes last so it's always on top during a rotate
 	-->

	<!-- The far left (index: -2) photo -->
	<div class="photoContainer" style="left: {$tweenedMotionValues.at(-2).left}vw; width: {$tweenedMotionValues.at(-2).width}vw">
		<img 
			class="photo" 
			style="transform: translateZ({$tweenedMotionValues.at(-2).translateZ}vw) rotateY({$tweenedMotionValues.at(-2).rotateY}deg)"
			src={photos.at((currentIndex - 2) % photoCount).src} 
			alt={photos.at((currentIndex - 2) % photoCount).text} 
		/>
	</div>

	<!-- The far right (index: +2) photo -->
	<div class="photoContainer" style="left: {$tweenedMotionValues.at(+2).left}vw; width: {$tweenedMotionValues.at(+2).width}vw">
		<img 
			class="photo" 
			style="transform: translateZ({$tweenedMotionValues.at(+2).translateZ}vw) rotateY({$tweenedMotionValues.at(+2).rotateY}deg)"
			src={photos.at((currentIndex + 2) % photoCount).src} 
			alt={photos.at((currentIndex + 2) % photoCount).text} 
		/>
	</div>

	<!-- The left of center (index: -1) photo -->
	<!-- svelte-ignore a11y-click-events-have-key-events -->
	<div on:click={() => rotate(-1)} class="photoContainer plus-minus-1" style="left: {$tweenedMotionValues.at(-1).left}vw; width: {$tweenedMotionValues.at(-1).width}vw">
		<img 
			class="photo" 
			style="transform: translateZ({$tweenedMotionValues.at(-1).translateZ}vw) rotateY({$tweenedMotionValues.at(-1).rotateY}deg)"
			src={photos.at((currentIndex - 1) % photoCount).src} 
			alt={photos.at((currentIndex - 1) % photoCount).text} 
		/>
	</div>

	<!-- The right of center (index: +1) photo -->
	<!-- svelte-ignore a11y-click-events-have-key-events -->
	<div on:click={() => rotate(+1)} class="photoContainer plus-minus-1" style="left: {$tweenedMotionValues.at(+1).left}vw; width: {$tweenedMotionValues.at(+1).width}vw">
		<img 
			class="photo" 
			style="transform: translateZ({$tweenedMotionValues.at(+1).translateZ}vw) rotateY({$tweenedMotionValues.at(+1).rotateY}deg)"
			src={photos.at((currentIndex + 1) % photoCount).src} 
			alt={photos.at((currentIndex + 1) % photoCount).text} 
		/>
	</div>

	<!-- The center (index: 0) photo -->
	<!-- svelte-ignore a11y-mouse-events-have-key-events -->
	<div 
		class="photoContainer" 
		style="left: {$tweenedMotionValues.at(0).left}vw; width: {$tweenedMotionValues.at(0).width}vw" 			
	    on:mouseover={() => isHovered = true}
		on:mouseout={() => isHovered = false}
	>
		<img 
			class:blurred={isHovered}
			class="photo" 
			style="transform: translateZ({$tweenedMotionValues.at(0).translateZ}vw) rotateY({$tweenedMotionValues.at(0).rotateY}deg);"
			src={photos.at((currentIndex) % photoCount).src} 
			alt={photos.at((currentIndex) % photoCount).text} 
		/>
		
	</div>

</div>


<style>

	.carouselContainer {
		position: relative;
		width: 100vw;  /* the sum of the widths of the 5 photos: 0 + 20 + 60 + 20 + 0 vw wide */
		height: 50vw;
		perspective: 80vw;
	}

	.photoContainer {
		transform-style: preserve-3d;
		position: absolute;
		height: 40vw;
	}

	.plus-minus-1 {
		cursor: pointer;
	}
	
	.photo {
		position: relative;
		text-align: center;
		width: 100%;
		height: 100%;
		object-fit: fill;
	}

	#photo-text {
		color: var(--color-theme-1);
		font-size: 2.2vw;
	}

	/* .centered {
	  position: absolute;
	  top: 50%;
	  left: 50%;
	  transform: translate(-50%, -50%);
	} */

	.hidden {
		display: none;
	}

	/* .blurred {
		filter: blur(3px) brightness(50%);
	} */

</style>
