<script context="module" lang="ts">
	import type { Load } from "@sveltejs/kit";

	export const load: Load = async ({ url, fetch }) => ({
		props: {
			post: await fetch(`${ url.pathname }.json`).then(response => response.json())
		}
	});
</script>

<script lang="ts">
	import { Metadata, externalLink } from "$lib";
	import Share from "@fluentui/svg-icons/icons/share_24_regular.svg?raw";
	import ArrowLeft from "@fluentui/svg-icons/icons/arrow_left_24_regular.svg?raw";
	import { IconButton, MenuFlyout, MenuFlyoutItem } from "fluent-svelte";

	export let post: {
		metadata: {
			title: string;
			description: string;
			thumbnail: string;
			date: string;
			author: string;
		}
	};

	const { title, thumbnail, author, date } = post.metadata;
</script>

<svelte:head>
	<Metadata title="{title}" image={thumbnail} />
</svelte:head>

<section class="blog-post">
	<article>
		<div class="post-title">
			<IconButton
				--icon-color="var(--text-color-secondary)"
				aria-label="Back to Trends"
				class="back-button"
				href="/trends"
				title="Back to Trends"
			>
				{@html ArrowLeft}
			</IconButton>
			<h1>{title}</h1>
		</div>
		<div class="video-info">
			<img alt="{author} avatar" src="https://github.com/{author}.png">
			<a
				class="hyperlink"
				href="https://github.com/{author}"
				{...externalLink}>@{author}</a
			>
			<span>•</span>
			{new Date(date.replace(/-/g, "/").replace(/T.+/, "")).toLocaleDateString(
				"en-US",
				{
					year: "numeric",
					day: "numeric",
					month: "short"
				}
			)}
			<MenuFlyout placement="bottom">
				<IconButton size={20} aria-label="Share" class="share-button" title="Share">
					{@html Share}
				</IconButton>
				<svelte:fragment slot="flyout">
					<MenuFlyoutItem
						on:click={() => navigator.clipboard.writeText(window.location.href)}
					>
						Copy URL
					</MenuFlyoutItem>
				</svelte:fragment>
			</MenuFlyout>
		</div>
		{#if thumbnail}
			<img class="post-thumbnail" src={thumbnail} alt="Thumbnail">
		{/if}
		<div class="markdown-body">
			<slot />
		</div>
	</article>
</section>

<style lang="scss">
	@use "../src/styles/pages/video.scss";
</style>