<script lang="ts">
	import { goto } from '$app/navigation';
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
	<div class="flex items-center">
		<Button variant="ghost" on:click={() => goto('/')}><Home /></Button>
		<Button on:click={toggleMode} variant="ghost" size="icon">
			<Sun
				class="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0"
			/>
			<Moon
				class="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100"
			/>
			<span class="sr-only">Toggle theme</span>
		</Button>
	</div>
	<h3>Carlton's Utilities</h3>
	<div class="flex">
		<Button
			variant="ghost"
			on:click={appWindow.minimize}
			class="px-2 rounded-full"
		>
			<MinusCircle />
		</Button>
		<Button
			variant="ghost"
			on:click={appWindow.toggleMaximize}
			class="px-2 rounded-full"
		>
			<PlusCircle />
		</Button>
		<Button
			variant="ghost"
			on:click={async () => await exit(1)}
			class="px-2 rounded-full"
		>
			<XCircle />
		</Button>
	</div>
</nav>
