<script lang="ts">
	import { createEventDispatcher, getContext } from "svelte";
	import { scale, fade } from "svelte/transition";
	import { mode } from "../stores";
	import { modeData, timeRemaining } from "../utils";
	import GameIcon from "./GameIcon.svelte";
	import type { Toaster } from "./widgets";

	export let showStats: boolean;
	export let tutorial: boolean;
	export let showRefresh: boolean;

	export let toaster = getContext<Toaster>("toaster");

	const dispatch = createEventDispatcher();
	mode.subscribe((m) => {
		if (timeRemaining(modeData.modes[m]) > 0) {
			showRefresh = false;
		}
	});
</script>

<header>
	<h1>
		wordle cz
	</h1>
	{#if tutorial}
		<div transition:scale class="tutorial" on:click={() => dispatch("closeTutPopUp")}>
			Tap WORDLE+ to change game mode
			<span class="ok">OK</span>
		</div>
	{/if}
</header>

<style lang="scss">
	header {
		--height: 51px;
		position: relative;
		font-weight: 700;
		text-transform: uppercase;
		letter-spacing: 0.2rem;
		display: flex;
		justify-content: space-between;
		align-items: center;
		border-bottom: 1px solid var(--border-primary);
		width: 100%;
		height: var(--height);
		position: relative;
	}
	.icons {
		height: 100%;
		z-index: 1;
		display: flex;
	}
	h1 {
		position: absolute;
		width: max-content;
		left: 50%;
		transform: translateX(-50%);
		font-size: var(--fs-large);
		cursor: pointer;
		text-align: center;
	}
</style>
