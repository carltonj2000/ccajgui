<script lang="ts">
	import { Collapsible } from 'bits-ui';
	import { ChevronDown as CaretUpDown } from 'lucide-svelte';
	import { slide } from 'svelte/transition';

	import { createCollapsible, melt } from '@melt-ui/svelte';
	const {
		elements: { root, content, trigger },
		states: { open }
	} = createCollapsible();
	console.log({ melt });
</script>

<div class="flex flex-col items-center">
	<!-- In order to following to work you have install and configure the melt pre processor-->
	<div use:melt={$root}>
		<button use:melt={$trigger}>{$open ? 'Close' : 'Open'}</button>
		{#if $open}
			<div use:melt={$content}>Obi-Wan says: Hello there!</div>
		{/if}
	</div>

	<Collapsible.Root class="w-[327px] space-y-3">
		<div class="flex items-center justify-between space-x-10">
			<h4 class="text-[15px] font-medium">@huntabyte starred 3 repositories</h4>
			<Collapsible.Trigger
				class="rounded-9px border-border-input shadow-btn active:scale-98 inline-flex h-10 w-10 items-center justify-center border bg-background text-foreground transition-all hover:bg-muted"
			>
				<CaretUpDown class="sq-4 " weight="bold" />
				<span class="sr-only">Toggle</span>
			</Collapsible.Trigger>
		</div>

		<Collapsible.Content
			class="space-y-2 font-mono text-[15px] tracking-[0.01em]"
			transition={slide}
		>
			<div
				class="rounded-9px inline-flex h-12 w-full items-center bg-muted px-[18px] py-3"
			>
				@huntabyte/bits-ui
			</div>
			<div
				class="rounded-9px inline-flex h-12 w-full items-center bg-muted px-[18px] py-3"
			>
				@huntabyte/shadcn-svelte
			</div>
			<div
				class="rounded-9px inline-flex h-12 w-full items-center bg-muted px-[18px] py-3"
			>
				@melt-ui/melt-ui
			</div>
		</Collapsible.Content>
	</Collapsible.Root>
</div>
