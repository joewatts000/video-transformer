<script lang="ts">
	import { onMount } from "svelte";

	let loaded = false;
	let video: HTMLVideoElement;
	let iframe: HTMLIFrameElement;
	let image: HTMLImageElement;

	const isImage = (url: string) => {
		return url.match(/\.(jpeg|jpg|gif|png)$/) != null;
	};

	const isIframe = (url: string) => {
		return url.includes('.m3u8');
	};

	const handleUrlChange = (event: any) => {
		event.preventDefault();
		const url = event.target.value;
		const isFrame = isIframe(url);
		const isImg = isImage(url);
		if (isFrame) {
			video.style.display = 'none';
			image.style.display = 'none';
			loaded = false;
			iframe.onload = () => {
				console.log('hi');
				loaded = true;
			};
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

	const changeBrightness = (event: any) => {
		const brightness = event.target.value;
		video.style.filter = `brightness(${brightness}%)`;
		iframe.style.filter = `brightness(${brightness}%)`;
		image.style.filter = `brightness(${brightness}%)`;
	};

	const changeContrast = (event: any) => {
		const contrast = event.target.value;
		video.style.filter = `contrast(${contrast}%)`;
		iframe.style.filter = `contrast(${contrast}%)`;
		image.style.filter = `contrast(${contrast}%)`;
	};

	const changeSaturation = (event: any) => {
		const saturation = event.target.value;
		video.style.filter = `saturate(${saturation}%)`;
		iframe.style.filter = `saturate(${saturation}%)`;
		image.style.filter = `saturate(${saturation}%)`;
	};

	const changeHue = (event: any) => {
		const hue = event.target.value;
		video.style.filter = `hue-rotate(${hue}deg)`;
		iframe.style.filter = `hue-rotate(${hue}deg)`;
		image.style.filter = `hue-rotate(${hue}deg)`;
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
		<div>
			<label for="video">Media</label>
			<input class="url-input" type="text" placeholder="Enter a url" on:change={handleUrlChange} on:input={handleUrlChange} />
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
		</div>
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
		margin-bottom: 1rem;
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
	}

	.input-box {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 1rem;
	}

</style>
