<script>
	let slides = [
		{
			src: '/clip1.mp4',
			captions: [
				{ time: 0, text: '¡Bienvenidos al espectáculo de cine mudo!' },
				{ time: 5, text: '¡Disfruten esta escena de acción impresionante!' }
			],
			description: 'Un misterioso héroe aparece en la escena.'
		},
		{
			src: '/clip2.mp4',
			captions: [
				{ time: 0, text: '¿Qué sucederá después?' },
				{ time: 4, text: 'Un giro inesperado está por llegar...' }
			],
			description: 'Un misterioso héroe aparece en la escena.'
		}
	];
	let currentSlideIndex = 0;
	let currentCaption = '';
	let showDescription = true; // Only used for initial display
	let videoElement;

	function updateCaption() {
		const video = slides[currentSlideIndex];
		if (video && video.captions) {
			const time = videoElement?.currentTime || 0;
			const caption = video.captions.find((c) => time >= c.time);
			currentCaption = caption ? caption.text : '';
		}
	}

	function nextSlide() {
		if (currentSlideIndex < slides.length - 1) {
			currentSlideIndex++;
			showDescription = false; // Hide description after initial display
			currentCaption = '';
		}
	}

	function previousSlide() {
		if (currentSlideIndex > 0) {
			currentSlideIndex--;
			showDescription = true; // Show description when navigating back
			currentCaption = '';
		}
	}

	function startVideo() {
		videoElement.play();
	}

	window.onload = function () {
		videoElement = document.querySelector('video');
		videoElement.onended = nextSlide;
		videoElement.ontimeupdate = updateCaption;
		// Start the first video automatically
		startVideo();
	};
</script>

<div class="presentation relative flex h-screen w-full items-center justify-center bg-black">
	<!-- svelte-ignore a11y_media_has_caption -->
	<video
		bind:this={videoElement}
		src={slides[currentSlideIndex].src}
		autoplay
		controls
		preload="auto"
		class="h-full w-full object-cover"
		ontimeupdate={updateCaption}
	></video>

	<div class="absolute bottom-16 left-1/2 -translate-x-1/2 transform text-center">
		<p class="rounded-md bg-black bg-opacity-75 px-4 py-2 text-lg text-white">
			{currentCaption}
		</p>
	</div>

	<div class="absolute inset-0 flex hidden flex-col items-center justify-center px-4 text-center">
		<p class="mb-4 text-2xl font-bold text-white">
			{slides[currentSlideIndex].description}
		</p>
	</div>

	<div class="absolute right-4 top-4 flex gap-2">
		<button
			onclick={previousSlide}
			disabled={currentSlideIndex === 0}
			class="rounded-full bg-black px-4 py-2 text-sm text-white hover:bg-gray-700 disabled:cursor-not-allowed disabled:bg-gray-500"
		>
			Anterior
		</button>
		<button
			onclick={nextSlide}
			disabled={currentSlideIndex === slides.length - 1}
			class="rounded-full bg-black px-4 py-2 text-sm text-white hover:bg-gray-700 disabled:cursor-not-allowed disabled:bg-gray-500"
		>
			Siguiente
		</button>
	</div>
</div>
