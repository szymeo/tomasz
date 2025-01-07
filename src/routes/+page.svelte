<script lang="ts">
	import AnimatedLayer from '$lib/AnimatedLayer.svelte';
	import { Sprite, Stage, Text } from 'glixy';

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
	class="fixed left-10 top-10 w-72 rounded-3xl border-r border-t border-white/10 bg-white/20 p-3 text-white backdrop-blur-lg"
>
	<h1 class="mb-3 p-4 text-center font-mono text-2xl font-bold">Wallpapers, yay!</h1>

	<div class="flex flex-col gap-3">
		{#each wallpapers as wallpaper}
			<img
				onmouseenter={() => (previewedWallpaper = wallpaper)}
				onmouseleave={() => {
					if (previewedWallpaper === wallpaper) {
						previewedWallpaper = null;
					}
				}}
				class="aspect-video h-auto w-full rounded-2xl object-cover"
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

	<AnimatedLayer visible={!previewedWallpaper} transition={{ duration: 25 }}>
		{#snippet children(opacity: number)}
			<Text
				text="Hover over a wallpaper to preview it!"
				style={{
					color: 0xffffff,
					font: 'Arial',
					size: 54,
					style: 'normal',
					weight: 'bold'
				}}
				anchor={{ x: 0.5, y: 0.5 }}
				width={200}
				x={width / 2}
				y={height / 2}
				{opacity}
			/>
		{/snippet}
	</AnimatedLayer>
</Stage>
