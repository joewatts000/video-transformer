<script lang="ts">
	import { onMount } from "svelte";

	let loaded = false;
	let video: HTMLVideoElement;
	let iframe: HTMLIFrameElement;
	let image: HTMLImageElement;
	// use a map to store the filters
	const filtersMap = new Map();

	const isImage = (url: string) => url.match(/\.(jpeg|jpg|gif|png)$/) != null;
	const isIframe = (url: string) => url.includes('.m3u8');

	const handleUrlInputChange = (event: any) => {
		event.preventDefault();
		const url = event.target.value;
		const isFrame = isIframe(url);
		const isImg = isImage(url);
		if (isFrame) {
			video.style.display = 'none';
			image.style.display = 'none';
			loaded = false;
			iframe.onload = () => loaded = true;
			iframe.style.display = 'block';
			iframe.src = `https://www.hlsplayer.org/play?url=${url}`;
		} else if (isImg) {
			video.style.display = 'none';
			iframe.style.display = 'none';
			image.style.display = 'block';
			image.src = url;
		} else {
			video.style.display = 'block';
			image.style.display = 'none';
			iframe.style.display = 'none';
			video.src = url;
		}
	};

	const updateElementsWithFilters = () => {
		const filters = Array.from(filtersMap.entries()).map(([key, value]) => `${key}(${value})`).join(' ');
		video.style.filter = filters;
		iframe.style.filter = filters;
		image.style.filter = filters;
	};

	const changeBrightness = (event: any) => {
		const brightness = event.target.value;
		filtersMap.set('brightness', `${brightness}%`);
		updateElementsWithFilters();
	};

	const changeContrast = (event: any) => {
		const contrast = event.target.value;
		filtersMap.set('contrast', `${contrast}%`);
		updateElementsWithFilters();
	};

	const changeSaturation = (event: any) => {
		const saturation = event.target.value;
		filtersMap.set('saturate', `${saturation}%`);
		updateElementsWithFilters();
	};

	const changeHue = (event: any) => {
		const hue = event.target.value;
		filtersMap.set('hue-rotate', `${hue}deg`);
		updateElementsWithFilters();
	};

	const changeBlur = (event: any) => {
		const blur = event.target.value;
		filtersMap.set('blur', `${blur}px`);
		updateElementsWithFilters();
	};

	const changeDropShadow = (event: any) => {
		const dropShadow = event.target.value;
		filtersMap.set('drop-shadow', `${dropShadow}px ${dropShadow}px ${dropShadow}px black`);
		updateElementsWithFilters();
	};

	const changeGrayscale = (event: any) => {
		const grayscale = event.target.value;
		filtersMap.set('grayscale', `${grayscale}%`);
		updateElementsWithFilters();
	};

	const changeInvert = (event: any) => {
		const invert = event.target.value;
		filtersMap.set('invert', `${invert}%`);
		updateElementsWithFilters();
	};

	const changeOpacity = (event: any) => {
		const opacity = event.target.value;
		filtersMap.set('opacity', `${opacity}%`);
		updateElementsWithFilters();
	};

	const changeSepia = (event: any) => {
		const sepia = event.target.value;
		filtersMap.set('sepia', `${sepia}%`);
		updateElementsWithFilters();
	};

	const clearAllFilters = () => {
		filtersMap.clear();
		video.style.filter = '';
		iframe.style.filter = '';
		image.style.filter = '';
	};

	onMount(() => {
		loaded = true;
		video = document.getElementById('video') as HTMLVideoElement;
		iframe = document.getElementById('iframe') as HTMLIFrameElement;
		image = document.getElementById('image') as HTMLImageElement;
	});
</script>

<svelte:head>
	<title>Media Transformer</title>
	<meta name="description" content="Media transformer app" />
</svelte:head>

<section>
	<h1>Media Transformer</h1>
	<h2>Transform your media with CSS filters</h2>
	<p>Enter a video, image or iframe url to begin</p>
	<p>Reset at any time. When happy export your tranformed media</p>
	<p>Save your preferences for easy conversion of multiple files</p>
	<div class="media-input-wrap">
		<label for="video">Media</label>
		<input class="url-input" type="text" placeholder="Enter a url" on:change={handleUrlInputChange} />
	</div>
	<div class="inputs">
		<div class="input-box">
			<label for="brightness">Brightness</label>
			<input type="range" min="0" max="500" value="100" id="brightness" on:change={changeBrightness} on:input={changeBrightness} />
		</div>
		<div class="input-box">
			<label for="contrast">Contrast</label>
			<input type="range" min="0" max="500" value="100" id="contrast" on:change={changeContrast} on:input={changeContrast} />
		</div>
		<div class="input-box">
			<label for="saturation">Saturation</label>
			<input type="range" min="0" max="500" value="100" id="saturation" on:change={changeSaturation} on:input={changeSaturation} />
		</div>
		<div class="input-box">
			<label for="hue">Hue</label>
			<input type="range" min="0" max="360" value="0" id="hue" on:change={changeHue} on:input={changeHue} />
		</div>
		<div class="input-box">
			<label for="blur">Blur</label>
			<input type="range" min="0" max="10" value="0" id="blur" on:change={changeBlur} on:input={changeBlur} />
		</div>
		<div class="input-box">
			<label for="drop-shadow">Drop Shadow</label>
			<input type="range" min="0" max="50" value="0" id="drop-shadow" on:change={changeDropShadow} on:input={changeDropShadow} />
		</div>
		<div class="input-box">
			<label for="grayscale">Grayscale</label>
			<input type="range" min="0" max="100" value="0" id="grayscale" on:change={changeGrayscale} on:input={changeGrayscale} />
		</div>
		<div class="input-box">
			<label for="invert">Invert</label>
			<input type="range" min="0" max="100" value="0" id="invert" on:change={changeInvert} on:input={changeInvert} />
		</div>
		<div class="input-box">
			<label for="opacity">Opacity</label>
			<input type="range" min="0" max="100" value="100" id="opacity" on:change={changeOpacity} on:input={changeOpacity} />
		</div>
		<div class="input-box">
			<label for="sepia">Sepia</label>
			<input type="range" min="0" max="100" value="0" id="sepia" on:change={changeSepia} on:input={changeSepia} />
		</div>
	</div>
	<button class="clear-button" on:click={clearAllFilters}>Reset</button>
	{#if !loaded}
		<div class="loader"></div>
	{/if}
	<video id="video" controls>
		<source src="" type="video/mp4" />
		Your browser does not support the video tag.
		<track kind="captions" />
	</video>
	<iframe id="iframe" src="" frameBorder="0" width="100%" allowFullScreen title=""></iframe>
	<img id="image" src="" alt="" />
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
		margin-bottom: 2rem;
	}

	.loader {
		margin: 80px auto;
		width: 45px;
		aspect-ratio: 1;
		--c: no-repeat linear-gradient(var(--color-text) 0 0);
		background:
			var(--c) 0%   100%,
			var(--c) 50%  100%,
			var(--c) 100% 100%;
		animation: l2 1s infinite linear;
	}
	@keyframes l2 {
		0%  {background-size: 20% 100%,20% 100%,20% 100%}
		20% {background-size: 20% 60% ,20% 100%,20% 100%}
		40% {background-size: 20% 80% ,20% 60% ,20% 100%}
		60% {background-size: 20% 100%,20% 80% ,20% 60% }
		80% {background-size: 20% 100%,20% 100%,20% 80% }
		100%{background-size: 20% 100%,20% 100%,20% 100%}
	}

	.url-input {
		margin-left: 1rem;
    padding: 0.8rem;
    border-radius: 4px;
    border: 0;
	}

	video {
		display: none;
		width: 100%;
	}

	iframe {
		aspect-ratio: 16 / 9;
		width: 100%;
		max-width: 100%;
		display: none;
	}

	#image {
		display: none;
		width: 100%;
	}

	.inputs {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		padding: 1rem;
		gap: 1rem;
		flex-wrap: wrap;
		padding-top: 3rem;
	}

	.input-box {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 1rem;
	}

	.media-input-wrap {
		padding: 1rem 0;
		border-top: 1px solid #ccc;
		border-bottom: 1px solid #ccc;
		width: 100%;
    text-align: center;
		max-width: 700px;
	}
	.clear-button {
		margin-bottom: 2rem;
	}

</style>
