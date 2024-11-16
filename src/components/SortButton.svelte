<script>
	import { ButtonGroup, Button, Dropdown, DropdownItem, Radio } from 'flowbite-svelte';
	import {
		UserCircleSolid,
		AdjustmentsVerticalOutline,
		DownloadSolid,
		ChevronDownOutline,
		GridSolid,
		ListOutline
	} from 'flowbite-svelte-icons';
	import { get } from 'svelte/store';

	let { group } = $props();

	$effect(() => {
		console.log('GroupSort', group);
		return () => {
			group;
		};
	});
</script>

<div class="flex items-center justify-between">
	<ButtonGroup>
		<Button outline color="dark">
			<ListOutline class="me-2 h-4 w-4 checked:text-blue-500" />
		</Button>
		<Button outline color="dark">
			<GridSolid class="me-2 h-4 w-4" />
		</Button>
	</ButtonGroup>

	<Button
		class="border-1 flex gap-2 border border-gray-400 bg-gray-200 text-black hover:bg-gray-300 focus:text-blue-700 focus:ring-2 focus:ring-gray-200"
		><AdjustmentsVerticalOutline
			class="h-6 w-6 text-black focus:text-blue-700 dark:text-white"
		/>Filter By Status<ChevronDownOutline
			class=" h-6 w-6 text-black focus:text-blue-700 dark:text-white"
		/></Button
	>
	<Dropdown class="w-48 space-y-1 p-3 ">
		<li class="rounded p-2 hover:bg-gray-100 dark:hover:bg-gray-600">
			<Radio
				name="group"
				class="focus:text-indigo-600 focus:ring-2 focus:ring-indigo-600"
				bind:group
				value="active">Active</Radio
			>
		</li>
		<li class="rounded p-2 hover:bg-gray-100 dark:hover:bg-gray-600">
			<Radio name="group" bind:group value="retired">Retired</Radio>
		</li>
		<li class="rounded p-2 hover:bg-gray-100 dark:hover:bg-gray-600">
			<Radio name="group" bind:group value="under construction">Under Construction</Radio>
		</li>
	</Dropdown>
</div>
