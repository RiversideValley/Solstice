<script context="module" lang="ts">
	import type { ErrorLoad } from "@sveltejs/kit";

	export const load: ErrorLoad = ({ error, status }) => {
		return {
			props: {
				status: status,
				error: error
			}
		};
	};
</script>

<script lang="ts">
	import { ProgressBar, Button, ContentDialog, TextBlock } from "fluent-svelte";
	import { Metadata } from "$lib";


	export let error: Error;
</script>

<svelte:head>
	<Metadata />
</svelte:head>

<section class="error-page">
	<ContentDialog open title="Uh Oh!">
		Looks like something went wrong.
		<br/>
		<TextBlock variant="bodyStrong">
			{error.message}
		</TextBlock>
		<svelte:fragment slot="footer">
		<Button href="/">
			Return Home
		</Button>
	</svelte:fragment>
	</ContentDialog>
</section>

<style lang="scss">
	@use "src/styles/pages/error";
</style>
