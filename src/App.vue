<script setup>
import GIF from 'gif.js.optimized';

var gif = new GIF({
	workers: 2,
	quality: 10,
	width: 600,
	height: 392,
	workerScript: './js/gifjs/gif.worker.js'
});

// Array of image file paths
var images = ['./images/test/image1.jpg',
			  './images/test/image2.jpg',
			  './images/test/image3.jpg',
			  './images/test/image4.jpg'];

// Helper function to load images
function loadImage(src, callback) {
	var img = new Image();
	img.src = src;
	img.onload = function () {
		console.log('Image loaded:', src);  // Log when image is loaded
		callback(img);
	};
	img.onerror = function () {
		console.error('Failed to load image:', src);
	};
}

var loadedImagesCount = 0;

// Load and add each image as a frame to the GIF
images.forEach(function (src) {
	loadImage(src, function (img) {
		gif.addFrame(img, { delay: 500 });
		loadedImagesCount++;
		// Check if all images are loaded
		if (loadedImagesCount === images.length) {
			console.log('All images loaded, starting GIF rendering...');
			gif.render();
		}
	});
});

// Render the GIF when all frames are added
gif.on('finished', function (blob) {
	console.log('GIF rendering finished.');
	document.getElementById('gif-result').src = URL.createObjectURL(blob);
});
</script>

<template>
	<h1>gif.js on Vue</h1>

	<div id="gif-container">
		<!-- This is where the generated GIF will be displayed -->
		<img id="gif-result" alt="Generated GIF will appear here">
	</div>
</template>

<style scoped>
.logo {
	height: 6em;
	padding: 1.5em;
	will-change: filter;
	transition: filter 300ms;
}

.logo:hover {
	filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
	filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
