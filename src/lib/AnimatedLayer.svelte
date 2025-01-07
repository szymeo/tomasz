<script lang="ts">
	import type { Snippet } from 'svelte';

	type Props = {
		visible: boolean;
		transition: { duration: number; easing?: (t: number) => number };
		children: Snippet<[opacity: number]>;
	};

	const { children, visible, transition }: Props = $props();

	let opacity = $state(0);
	let animationFrameId = null;

	$effect(() => {
		if (animationFrameId) {
			cancelAnimationFrame(animationFrameId);
		}

		let start = null;
		const duration = transition.duration;
		const easing = transition.easing || ((t) => t);
		const initialOpacity = opacity;
		const targetOpacity = visible ? 1 : 0;

		function step(timestamp) {
			if (!start) {
				start = timestamp;
			}
			const progress = Math.min((timestamp - start) / duration, 1);
			opacity = initialOpacity + (targetOpacity - initialOpacity) * easing(progress);

			if (progress < 1) {
				animationFrameId = requestAnimationFrame(step);
			} else {
				animationFrameId = null;
			}
		}

		animationFrameId = requestAnimationFrame(step);
	});
</script>

{@render children(opacity)}
