<script>

    // @ts-nocheck

	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

	/**
	 * @type {object[]}
	 */
	export let photos = [];
	
	export let duration = 500;  // The duration of the animation in milliseconds
	export let easing = cubicOut;  // The easing function to use for the animation

	const options = {duration, easing};

	let photoCount = photos.length;
	if (photoCount < 1) {
		throw new Error('PhotoCarousel requires at least one photo');
	}
	let currentIndex = 0;
	let isHovered = false;

	// There are 3 visible slots in the carousel but you can have as many as you want in the photos array.
	// If you have less than 3 photos, the photos will repeat to fill up the 3 visible (and 2 invisible) slots.

	// There are 2 additional slots, far left and far right.
	// They are turned perpendicular to the screen so they are invisible.
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
		{translateZ: 0, rotateY: 0, left: 210, width: 600},  // center 
		{translateZ: -60, rotateY: 30, left: 810, width: 200},  // center + 1 = right
		{translateZ: -170, rotateY: 60, left: 1010, width: 10},  // center + 2 = far right
		{translateZ: -170, rotateY: -60, left: 0, width: 10},  // center - 2 = far left
		{translateZ: -60, rotateY: -30, left: 10, width: 200},  // center - 1 = left
	]

	// The following section sets up the Svelte "tweens" which will animate the photos in the carousel
	const minus2TranslateZ = tweened(motionValues.at(-2).translateZ, options);
	const minus2RotateY = tweened(motionValues.at(-2).rotateY, options);
	const minus2Left = tweened(motionValues.at(-2).left, options);
	const minus2Width = tweened(motionValues.at(-2).width, options);
	const minus1TranslateZ = tweened(motionValues.at(-1).translateZ, options);
	const minus1RotateY = tweened(motionValues.at(-1).rotateY, options);
	const minus1Left = tweened(motionValues.at(-1).left, options);
	const minus1Width = tweened(motionValues.at(-1).width, options);
	const centerTranslateZ = tweened(motionValues.at(0).translateZ, options);
	const centerRotateY = tweened(motionValues.at(0).rotateY, options);
	const centerLeft = tweened(motionValues.at(0).left, options);
	const centerWidth = tweened(motionValues.at(0).width, options);
	const plus1TranslateZ = tweened(motionValues.at(+1).translateZ, options);
	const plus1RotateY = tweened(motionValues.at(+1).rotateY, options);
	const plus1Left = tweened(motionValues.at(+1).left, options);
	const plus1Width = tweened(motionValues.at(+1).width, options);
	const plus2TranslateZ = tweened(motionValues.at(+2).translateZ, options);
	const plus2RotateY = tweened(motionValues.at(+2).rotateY, options);
	const plus2Left = tweened(motionValues.at(+2).left, options);
	const plus2Width = tweened(motionValues.at(+2).width, options);

	function rotate(delta) {
		isHovered = false;  // Not necessary until/unless we add swipe and/or keystroke support

		// The following section sets the new motion values for the photos in the carousel and starts the animation
		// The use of the remainder operator, %, adjusts the index so it's not out of bounds
		minus2TranslateZ.set(motionValues.at((5 - delta - 2) % 5).translateZ);
		minus2RotateY.set(motionValues.at((5 - delta - 2) % 5).rotateY);
		minus2Left.set(motionValues.at((5 - delta - 2) % 5).left);
		minus2Width.set(motionValues.at((5 - delta - 2) % 5).width);
		minus1TranslateZ.set(motionValues.at((5 - delta - 1) % 5).translateZ);
		minus1RotateY.set(motionValues.at((5 - delta - 1) % 5).rotateY);
		minus1Left.set(motionValues.at((5 - delta - 1) % 5).left);
		minus1Width.set(motionValues.at((5 - delta - 1) % 5).width);
		centerTranslateZ.set(motionValues.at((5 - delta + 0) % 5).translateZ);
		centerRotateY.set(motionValues.at((5 - delta + 0) % 5).rotateY);
		centerLeft.set(motionValues.at((5 - delta + 0) % 5).left);
		centerWidth.set(motionValues.at((5 - delta + 0) % 5).width);
		plus1TranslateZ.set(motionValues.at((5 - delta + 1) % 5).translateZ);
		plus1RotateY.set(motionValues.at((5 - delta + 1) % 5).rotateY);
		plus1Left.set(motionValues.at((5 - delta + 1) % 5).left);
		plus1Width.set(motionValues.at((5 - delta + 1) % 5).width);
		plus2TranslateZ.set(motionValues.at((5 - delta + 2) % 5).translateZ);
		plus2RotateY.set(motionValues.at((5 - delta + 2) % 5).rotateY);
		plus2Left.set(motionValues.at((5 - delta + 2) % 5).left);
		plus2Width.set(motionValues.at((5 - delta + 2) % 5).width);

		// Once the above animation finishes, the following section resets everything to the starting position
		// and at the same instant shifts the index so everything is in the expected slot for the next user click.
		// Sometimes there is a slight flicker for this step, but it is not noticeable unless you are looking for it.
		// There was probably a better way to do this, but I couldn't figure it out.
		setTimeout(() => {
			currentIndex = currentIndex + delta;
			minus2TranslateZ.set(motionValues.at(-2).translateZ, {duration: 0});
			minus2RotateY.set(motionValues.at(-2).rotateY, {duration: 0});
			minus2Left.set(motionValues.at(-2).left, {duration: 0});
			minus2Width.set(motionValues.at(-2).width, {duration: 0});
			minus1TranslateZ.set(motionValues.at(-1).translateZ, {duration: 0});
			minus1RotateY.set(motionValues.at(-1).rotateY, {duration: 0});
			minus1Left.set(motionValues.at(-1).left, {duration: 0});
			minus1Width.set(motionValues.at(-1).width, {duration: 0});
			centerTranslateZ.set(motionValues.at(0).translateZ, {duration: 0});
			centerRotateY.set(motionValues.at(0).rotateY, {duration: 0});
			centerLeft.set(motionValues.at(0).left, {duration: 0});
			centerWidth.set(motionValues.at(0).width, {duration: 0});
			plus1TranslateZ.set(motionValues.at(+1).translateZ, {duration: 0});
			plus1RotateY.set(motionValues.at(+1).rotateY, {duration: 0});
			plus1Left.set(motionValues.at(+1).left, {duration: 0});
			plus1Width.set(motionValues.at(+1).width, {duration: 0});
			plus2TranslateZ.set(motionValues.at(+2).translateZ, {duration: 0});
			plus2RotateY.set(motionValues.at(+2).rotateY, {duration: 0});
			plus2Left.set(motionValues.at(+2).left, {duration: 0});
			plus2Width.set(motionValues.at(+2).width, {duration: 0});
		}, duration);
	}

</script>


<div class="carouselContainer">
	
	<!-- 
	Since the below divs are positioned absolutely, they can be in any order.
	So, first come the far left and far right divs that are essentially invisible except
	when the carousel rotates. By placing them first, they are behind the other divs.
	Then come the divs immediately to left and right of center.
	Finally, the center comes last so it's always on top during a rotate
 	-->

	<!-- The far left (index: -2) photo -->
	<div class="photoContainer" style="left: {$minus2Left}px; width: {$minus2Width}px">
		<img 
			class="photo" 
			style="transform: translateZ({$minus2TranslateZ}px) rotateY({$minus2RotateY}deg)"
			src={photos.at((currentIndex - 2) % photoCount).src} 
			alt={photos.at((currentIndex - 2) % photoCount).text} 
		/>
	</div>

	<!-- The far right (index: +2) photo -->
	<div class="photoContainer" style="left: {$plus2Left}px; width: {$plus2Width}px">
		<img 
			class="photo" 
			style="transform: translateZ({$plus2TranslateZ}px) rotateY({$plus2RotateY}deg)"
			src={photos.at((currentIndex + 2) % photoCount).src} 
			alt={photos.at((currentIndex + 2) % photoCount).text} 
		/>
	</div>

	<!-- The left of center (index: -1) photo -->
	<!-- svelte-ignore a11y-click-events-have-key-events -->
	<div on:click={() => rotate(-1)} class="photoContainer" style="left: {$minus1Left}px; width: {$minus1Width}px">
		<img 
			class="photo" 
			style="transform: translateZ({$minus1TranslateZ}px) rotateY({$minus1RotateY}deg)"
			src={photos.at((currentIndex - 1) % photoCount).src} 
			alt={photos.at((currentIndex - 1) % photoCount).text} 
		/>
	</div>

	<!-- The right of center (index: +1) photo -->
	<!-- svelte-ignore a11y-click-events-have-key-events -->
	<div on:click={() => rotate(+1)} class="photoContainer" style="left: {$plus1Left}px; width: {$plus1Width}px">
		<img 
			class="photo" 
			style="transform: translateZ({$plus1TranslateZ}px) rotateY({$plus1RotateY}deg)"
			src={photos.at((currentIndex + 1) % photoCount).src} 
			alt={photos.at((currentIndex + 1) % photoCount).text} 
		/>
	</div>

	<!-- The center (index: 0) photo -->
	<div class="photoContainer" style="left: {$centerLeft}px; width: {$centerWidth}px">
		<!-- svelte-ignore a11y-mouse-events-have-key-events -->
		<img 
			on:mouseover={() => isHovered = true}
			on:mouseout={() => isHovered = false}
			class:blurred={isHovered}
			class="photo" 
			style="transform: translateZ({$centerTranslateZ}px) rotateY({$centerRotateY}deg);"
			src={photos.at((currentIndex) % photoCount).src} 
			alt={photos.at((currentIndex) % photoCount).text} 
		/>
		<div id="photo-text" class:hidden={!isHovered} class="centered">{photos.at((currentIndex) % photoCount).text}</div>
	</div>

</div>


<style>

	.carouselContainer {
		position: relative;
		width: 1020px;  /* the sum of the widths of the 5 photos: 10 + 200 + 600 + 200 + 10 px wide */
		height: 400px;
		perspective: 800px;
	}

	.photoContainer {
		transform-style: preserve-3d;
		position: absolute;
		height: 400px;
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
		font-size: 30px;
	}

	.centered {
	  position: absolute;
	  top: 50%;
	  left: 50%;
	  transform: translate(-50%, -50%);
	}

	.hidden {
		display: none;
	}

	.blurred {
		filter: blur(3px) brightness(50%);
	}

</style>
