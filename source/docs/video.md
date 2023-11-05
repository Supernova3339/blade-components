---
title: Video
description: Video
extends: _layouts.documentation
section: content
---

# Video {#video}

Video player powered by AlpineJS, PlyrJS, TailwindCSS, and Laravel Blade View Components.

<video id="player" class="w-full" controls>
    <source src="https://i.e-z.host/lxqvwa1l.mp4" type="video/mp4">
</video>

### Usage

```php
// in blade view

<x-video src="your-video-here" type="video/mp4" />
```


### Component

```php
// components/video.blade.php

@props(['src', 'type'])

<div x-data="{ isLoading: true }" class="bg-white rounded-lg shadow-lg overflow-hidden">
    <div class="relative aspect-video border rounded-md overflow-hidden bg-slate-100">
        <div x-show="isLoading" class="absolute inset-0 flex items-center justify-center bg-slate-800 flex-col gap-y-2 text-secondary">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="h-8 w-8 animate-spin">
                <path d="M21 12a9 9 0 1 1-6.219-8.56"></path>
            </svg>
            <p class="text-sm mt-1 text-white">Loading video...</p>
        </div>
        <div x-show="!isLoading" id="video-player-container" class="absolute inset-y-0 inset-x-0 w-full h-full">
            <video id="player" class="w-full" controls>
                <source x-bind:src="videoSrc" x-bind:type="videoType">
            </video>
        </div>
    </div>
</div>

<script>
    window.addEventListener('DOMContentLoaded', function () {
        const videoContainer = document.querySelector('.aspect-video');
        const player = document.getElementById('player');
        const videoPlayerContainer = document.getElementById('video-player-container');

        // Simulate loading delay
        setTimeout(function () {
            const videoSrc = @json($src); // Get the video source from the component parameter
            const videoType = @json($type); // Get the video type from the component parameter

            player.setAttribute('src', videoSrc);
            player.setAttribute('type', videoType); // Set the video type
            player.load();
            player.addEventListener('loadeddata', function () {
                videoPlayerContainer.style.display = 'block'; // Show the video container
                videoContainer.querySelector('.text-secondary').style.display = 'none';

                // Initialize Plyr player
                const plyrPlayer = new Plyr(player, {
                    controls: [
                        'play-large',
                        'restart',
                        'rewind',
                        'play',
                        'fast-forward',
                        'progress',
                        'current-time',
                        'duration',
                        'mute',
                        'volume',
                        'captions',
                        'settings',
                        'pip',
                        'airplay',
                        'fullscreen',
                        'quality',
                    ],
                });

                // Update the isLoading flag to hide the preloader
                Alpine.store('isLoading', false);
            });
        }, 2000); // Delay time for user experience enhancement
    });
</script>
```