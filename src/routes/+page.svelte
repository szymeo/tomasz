<script lang="ts">
	import AnimatedLayer from '$lib/AnimatedLayer.svelte';
	import { Sprite, Stage, Text } from 'glixy';
	import { fly } from 'svelte/transition';

	let width = $state(0);
	let height = $state(0);

	const wallpapers = [
		'/images/wallpaper1.webp',
		'/images/wallpaper2.webp',
		'/images/wallpaper3.webp'
	];
	let previewedWallpaper = $state(null);
</script>

<svelte:window bind:innerWidth={width} bind:innerHeight={height} />

<div
	class="fixed bottom-3 left-3 right-3 h-auto w-11/12 overflow-auto rounded-3xl border-r border-t border-white/10 bg-white/20 p-3 text-white backdrop-blur-lg sm:bottom-10 sm:left-10 sm:top-10 sm:w-72"
>
	<h1 class="sticky left-0 mb-3 p-4 text-left font-mono text-2xl font-bold sm:text-center">
		Wallpapers, yay!
	</h1>

	<div class="flex gap-3 sm:flex-col">
		{#each wallpapers as wallpaper}
			<img
				onmouseenter={() => (previewedWallpaper = wallpaper)}
				onmouseleave={() => {
					if (previewedWallpaper === wallpaper) {
						previewedWallpaper = null;
					}
				}}
				class="aspect-video h-full w-auto max-w-44 rounded-2xl object-cover sm:h-auto sm:w-full sm:max-w-full"
				alt={wallpaper}
				src={wallpaper}
				width={100}
				height={100}
			/>
		{/each}
	</div>
</div>

<Stage host={window.document.body}>
	{#each wallpapers as wallpaper}
		<AnimatedLayer visible={previewedWallpaper === wallpaper} transition={{ duration: 125 }}>
			{#snippet children(opacity: number)}
				<Sprite
					texture={wallpaper}
					x={0}
					y={0}
					z={previewedWallpaper === wallpaper ? 1 : 0}
					{opacity}
					{width}
					{height}
				/>
			{/snippet}
		</AnimatedLayer>
	{/each}

	{#if !previewedWallpaper}
		<h1
			in:fly={{ y: 5, duration: 250 }}
			out:fly={{ y: -5, duration: 250 }}
			class="fixed left-1/2 top-1/3 -translate-x-1/2 -translate-y-1/2 text-center font-mono text-4xl font-bold text-white sm:left-2/3 sm:top-1/2"
		>
			Hover over a wallpaper to preview it!
		</h1>
	{/if}
</Stage>
