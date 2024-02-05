<script lang="ts">
	import * as Select from '$lib/components/ui/select';
	import { Select as SelectB } from 'bits-ui';

	const items: Select.SelectItem[string] = ['dog', 'cat', 'bird'];
	const selectedIndex = 0;
	let selectedItem = items[selectedIndex];
	let selectedItem2 = items[selectedIndex];
</script>

<div class="flex flex-col items-center">
	<h4>Select Example</h4>

	<p class="mt-6">The {selectedItem} is selected.</p>
	<Select.Root
		onSelectedChange={(e) => (selectedItem = e ? e.value : selectedItem)}
		{items}
		selected={selectedItem}
	>
		<Select.Trigger class="w-[180px]">
			<Select.Value placeholder={selectedItem} />
		</Select.Trigger>
		<Select.Content>
			{#each items as item}
				<Select.Item value={item}>{item}</Select.Item>
			{/each}
		</Select.Content>
	</Select.Root>

	<p class="mt-6">The {selectedItem2} is selected.</p>
	<SelectB.Root name="selectedItem2" selected={items[selectedIndex]} {items}>
		<SelectB.Trigger>
			<SelectB.Value placeholder={items[selectedIndex]} />
		</SelectB.Trigger>

		<SelectB.Content>
			{#each items as item}
				<Select.Item value={item}>{item}</Select.Item>
			{/each}
		</SelectB.Content>
	</SelectB.Root>
</div>
