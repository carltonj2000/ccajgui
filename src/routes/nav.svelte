<script lang="ts">
	import { Button } from '$lib/components/ui/button';
	import { exit } from '@tauri-apps/api/process';
	import { appWindow } from '@tauri-apps/api/window';
	import {
		Home,
		MinusCircle,
		Moon,
		PlusCircle,
		Sun,
		XCircle
	} from 'lucide-svelte';
	import { toggleMode } from 'mode-watcher';
</script>

<nav class="py-2 px-3 flex justify-between items-center" data-tauri-drag-region>
	<div class="flex items-center gap-2">
		<a href="/"><Home /></a>
		<Button on:click={toggleMode} variant="outline" size="icon">
			<Sun
				class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0"
			/>
			<Moon
				class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100"
			/>
			<span class="sr-only">Toggle theme</span>
		</Button>
	</div>
	<p>Carlton's Utilities</p>
	<div class="flex gap-1">
		<button on:click={appWindow.minimize}><MinusCircle /></button>
		<button on:click={appWindow.toggleMaximize}><PlusCircle /></button>
		<button on:click={async () => await exit(1)}><XCircle /></button>
	</div>
</nav>
