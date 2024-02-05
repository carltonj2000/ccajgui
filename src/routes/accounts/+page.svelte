<script lang="ts">
	import {
		BaseDirectory,
		createDir,
		exists,
		readTextFile,
		removeFile,
		writeTextFile
	} from '@tauri-apps/api/fs';

	import { Button } from '$lib/components/ui/button';
	import * as Card from '$lib/components/ui/card';
	import { Input } from '$lib/components/ui/input';
	import * as Select from '$lib/components/ui/select';
	import * as Table from '$lib/components/ui/table';
	import { XSquare } from 'lucide-svelte';
	import type { accountsT, instT } from './accounts';
	import { aType, accounts as accountsIn } from './accounts';

	const fileName = 'accounts.json';
	let accounts: accountsT = [...accountsIn];
	let loading = true;
	function isNumeric(str: string) {
		if (typeof str != 'string') return false;
		return !isNaN(str) && !isNaN(parseFloat(str));
	}
	const sumCalc = () =>
		accounts.reduce(
			(acc, { amount }) => (isNumeric(amount) ? acc + parseFloat(amount) : acc),
			0
		);
	let sum = sumCalc();
	const loadAccounts = async () => {
		loading = true;
		try {
			if (!(await exists(fileName, { dir: BaseDirectory.AppConfig }))) {
				loading = false;
			} else {
				const text = await readTextFile(fileName, {
					dir: BaseDirectory.AppConfig
				});
				//console.log(text);
				accounts = JSON.parse(text);
				sum = sumCalc();
				loading = false;
			}
		} catch (err) {
			console.error({ err });
			loading = false;
		}
	};

	loadAccounts();
	const updateSum = () => (sum = sumCalc());
	const clearBalances = async () => {
		accounts.map((a) => (a.amount = '0'));
		accounts = accounts;
		await saveAccounts(accounts);
		updateSum();
	};
	let iAddShow = false;
	let newI: instT = {
		amount: '0',
		institution: 'Kraken',
		type: aType.cash
	};
	let warning = '';
	let warningTimer: number;
	const addI = async () => {
		const alreadyPresent = accounts.reduce((a, i) => {
			return a || i.institution.localeCompare(newI.institution) === 0;
		}, false);
		if (alreadyPresent) {
			warning = 'Institution Already Present In Accounts';
			if (warningTimer) clearTimeout(warningTimer);
			warningTimer = setTimeout(() => {
				warning = '';
				iAddShow = false;
			}, 2000);
		} else {
			accounts = [...accounts, { ...newI }];
			updateSum();
			await saveAccounts(accounts);
			iAddShow = false;
		}
	};
	let iDeleteShow = false;
	const deleteI = async (institution: string) => {
		const filtered = accounts.filter((a) => a.institution !== institution);
		accounts = [...filtered];
		iDeleteShow = false;
		await saveAccounts(accounts);
	};
	const saveAccounts = async (accounts: accountsT) => {
		// console.log({ accounts });
		try {
			await createDir('', { dir: BaseDirectory.AppConfig, recursive: true });
			if (await exists(fileName, { dir: BaseDirectory.AppConfig }))
				await removeFile(fileName, { dir: BaseDirectory.AppConfig });
			await writeTextFile(fileName, JSON.stringify(accounts, null, 2), {
				dir: BaseDirectory.AppConfig
			});
		} catch (err) {
			console.error({ err });
		}
	};

	const items: Select.SelectItem[string] = Object.values(aType);
	const selectedIndex = 0;
	let selectedItem = items[selectedIndex];
</script>

<div class="flex flex-col items-center gap-2">
	{#if loading}
		<p class="text-center">Loading ...</p>
	{:else}
		<Card.Root class="flex flex-col items-center justify-center">
			<Card.Header>
				<Card.Title>Investment And Bank Accounts</Card.Title>
			</Card.Header>
			<Card.Content>
				<Table.Root>
					<Table.Header>
						<Table.Row>
							<Table.Head class="institution">Institution</Table.Head>
							<Table.Head class="type">Type</Table.Head>
							<Table.Head class="balance">Balance</Table.Head>
						</Table.Row>
					</Table.Header>
					<Table.Body>
						{#each accounts as { institution, type, amount }, idx}
							<Table.Row>
								<Table.Cell>
									<div class="flex items-center gap-2">
										{#if iDeleteShow}
											<Button on:click={() => deleteI(institution)}>
												<XSquare />
											</Button>
										{/if}
										{institution}
									</div>
								</Table.Cell>
								<Table.Cell>{type}</Table.Cell>
								<Table.Cell>
									<Input
										on:keyup={updateSum}
										on:change={updateSum}
										bind:value={accounts[idx].amount}
										type="text"
										size={10}
										class="text-right"
									/>
								</Table.Cell>
							</Table.Row>
						{/each}
						<Table.Row>
							<Table.Head>Sum</Table.Head>
							<Table.Head />
							<Table.Head class="text-right">${sum}</Table.Head>
						</Table.Row>
					</Table.Body>
				</Table.Root>
			</Card.Content>
			<Card.Footer class="flex gap-1">
				{#if !iDeleteShow}
					{#if !iAddShow}
						<Button on:click={clearBalances}>Clear Balances</Button>
						<Button on:click={() => (iAddShow = true)}>
							Add New Institution
						</Button>
						<Button on:click={() => (iDeleteShow = true)}>
							Delete Institution
						</Button>
					{/if}
				{:else}
					<Button on:click={() => (iDeleteShow = false)}>Cancel Deletion</Button
					>
				{/if}
			</Card.Footer>
		</Card.Root>
		{#if iAddShow}
			<Card.Root>
				<Card.Header>
					<div class="flex flex-row items-center justify-center gap-1">
						<Button on:click={addI}>Add New Institution</Button>
						<Button on:click={() => (iAddShow = false)}>Cancel Add</Button>
					</div>
				</Card.Header>
				<Card.Content>
					<Table.Root>
						<Table.Header>
							<Table.Row>
								<Table.Head class="institution">Institution</Table.Head>
								<Table.Head class="type">Type</Table.Head>
								<Table.Head class="balance">Balance</Table.Head>
							</Table.Row>
						</Table.Header>
						<Table.Body>
							<Table.Row>
								<Table.Cell>
									<Input bind:value={newI.institution} size={15} />
								</Table.Cell>
								<Table.Cell class="text-right">
									<Select.Root
										onSelectedChange={(e) =>
											(newI.type = e ? e.value : selectedItem)}
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
								</Table.Cell>
								<Table.Cell>
									<Input bind:value={newI.amount} size={10} />
								</Table.Cell>
							</Table.Row>
						</Table.Body>
					</Table.Root>
				</Card.Content>
				<Card.Footer class="flex justify-center">
					{#if warning.length}
						<p>{warning}</p>
					{/if}
				</Card.Footer>
			</Card.Root>
		{/if}
	{/if}
</div>
