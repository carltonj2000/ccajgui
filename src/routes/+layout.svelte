<script>
	import { Separator } from '$lib/components/ui/separator';
	import { ModeWatcher } from 'mode-watcher';
	import '../app.css';
	import Nav from './nav.svelte';
</script>

<ModeWatcher />
<Nav />
<Separator class="mb-3" />
<slot />
