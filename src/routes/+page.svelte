<script>
	let slides = [
		{
			src: '/clip1.mp4',
			captions: [
				{
					time: 0,
					text: 'Por andar de pillo Maddox estropea el conjuro del brujo causando que los chihuahuereans de otros universos se transporten'
				},
				{ time: 5, text: '¡Disfruten esta escena de acción impresionante!' }
			],
			description: 'Previamente en Elizabeth Coss...'
		},
		{
			src: '/clip2.mp4',
			captions: [
				{ time: 0, text: '' },
				{ time: 4, text: 'Un giro inesperado está por llegar...' }
			],
			description: 'Pero eso tambien causo que el obtenga poderes misticos'
		},
		{
			src: '/clip3.mp4',
			captions: [
				{ time: 0, text: 'Eli inaugura su coffe cookie shop' },
				{ time: 4, text: 'Un giro inesperado está por llegar...' }
			],
			description: 'Tiempo actual'
		},
		{
			src: '/clip4.mp4',
			captions: [
				{ time: 0, text: 'Maddox: Cajhones!' },
				{ time: 4, text: 'Un giro inesperado está por llegar...' }
			],
			description:
				'Todo iba muy bien hasta que un dia Maddox veia su video musical favorito "La familia Madrigal'
		},
		{
			src: '/clip5.mp4',
			captions: [
				{ time: 0, text: 'Eli: Hora de ver el hobbit, sorry Maddox.' },
				{ time: 4, text: 'Un giro inesperado está por llegar...' }
			],
			description: 'Hasta que a su mami se le antojo ver una pelicula'
		},
		{
			src: '/clip6.mp4',
			captions: [
				{ time: 0, text: 'Maddox: Pero mami… como puriste? … Mis cajhoonesss… 😭' },
				{ time: 4, text: 'Un giro inesperado está por llegar...' }
			],
			description: 'Pero Maddox no lo tomó muy bien'
		},
		{
			src: '/clip7.mp4',
			captions: [
				{ time: 0, text: 'Eli: Pero que esta pasando?!' },
				{ time: 4, text: 'Un giro inesperado está por llegar...' }
			],
			description: 'Maddox sin querer activa sus poderes magicos y…'
		},
		{
			src: '/clip8.mp4',
			captions: [
				{
					time: 0,
					text: 'Eli: Pero que es este lugar? y Maddox donde esta?'
				},
				{ time: 4, text: 'Un giro inesperado está por llegar...' }
			],
			description: 'Crea un portal a otro universo que los absorbe y los transporta a...'
		},
		{
			src: '/clip9.mp4',
			captions: [
				{
					time: 0,
					text: 'Diego: Eli? Maddox? donde estan? Pero que esta cosa brillante? Ok Luna tenemos que pensar un plan y...'
				},
				{ time: 4, text: 'Un giro inesperado está por llegar...' }
			],
			description: 'Diego y luna oyeron todo el alboroto en la sala'
		},
		{
			src: '/clip10.mp4',
			captions: [
				{
					time: 0,
					text: 'Luna: Siganme los piquetulos!'
				},
				{ time: 4, text: 'Un giro inesperado está por llegar...' }
			],
			description: 'Pero Luna no es de las que se ponen a planear...'
		},
		{
			src: '/clip11.mp4',
			captions: [
				{
					time: 0,
					text: 'Luna: En donde estamos piquetulo? Diego: aqui tu puedes hablar?'
				},
				{ time: 4, text: 'Un giro inesperado está por llegar...' }
			],
			description: 'Asi que a Diego no le queda mas que seguirla por el portal'
		},
		{
			src: '/clip12.mp4',
			captions: [
				{
					time: 0,
					text: 'Astra: Continuara en navridad. Asi es yo tambien rompo la cuarta pared como el deadpool rrraaaww!'
				},
				{ time: 4, text: 'Un giro inesperado está por llegar...' }
			],
			description: 'Continuara...'
		}
	];

	let currentSlideIndex = 0;
	let currentCaption = '';
	let showDescription = true; // Muestra las descripciones entre videos
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
			showDescription = true;
			currentCaption = '';
		}
	}

	function previousSlide() {
		if (currentSlideIndex > 0) {
			currentSlideIndex--;
			showDescription = true;
			currentCaption = '';
		}
	}

	function startVideo() {
		showDescription = false;
		videoElement.play();
	}
</script>

<!-- Presentación principal -->
<div class="presentation relative flex h-screen w-full items-center justify-center bg-black">
	<!-- Video con subtítulos -->
	{#if !showDescription}
		<!-- svelte-ignore a11y_media_has_caption -->
		<video
			bind:this={videoElement}
			src={slides[currentSlideIndex].src}
			autoplay
			controls
			preload="auto"
			class="h-full w-full object-contain"
			onended={nextSlide}
			ontimeupdate={updateCaption}
		></video>

		<!-- Subtítulos sobre el video -->
		<div class="absolute bottom-16 left-1/2 -translate-x-1/2 transform text-center">
			<p class="rounded-md bg-black bg-opacity-75 px-4 py-2 text-lg text-white">
				{currentCaption}
			</p>
		</div>
	{/if}

	<!-- Descripciones entre videos -->
	{#if showDescription}
		<div class="absolute inset-0 flex flex-col items-center justify-center px-4 text-center">
			<p class="mb-4 text-2xl font-bold text-white">
				{slides[currentSlideIndex].description}
			</p>
			<button
				onclick={startVideo}
				class="rounded-full bg-black px-6 py-2 text-lg text-white hover:bg-gray-700"
			>
				Continuar
			</button>
		</div>
	{/if}

	<!-- Botones de navegación en la esquina superior derecha -->
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
