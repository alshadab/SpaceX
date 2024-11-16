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

	let group2 = 2;
</script>

<div class="flex w-[70%] items-center justify-between">
	<ButtonGroup>
		<Button outline color="dark">
			<ListOutline class="me-2 h-4 w-4 checked:text-blue-500" />
		</Button>
		<Button outline color="dark">
			<GridSolid class="me-2 h-4 w-4" />
		</Button>
	</ButtonGroup>

	<Button
		class="border-1 flex gap-2 border border-gray-400 bg-gray-200 text-black hover:bg-gray-300"
		><AdjustmentsVerticalOutline class="h-6 w-6 text-black dark:text-white" />Filter By Status<ChevronDownOutline
			class=" h-6 w-6 text-black dark:text-white"
		/></Button
	>
	<Dropdown class="w-48 space-y-1 p-3">
		<li class="rounded p-2 hover:bg-gray-100 dark:hover:bg-gray-600">
			<Radio name="group2" bind:group={group2} value={1}>Active</Radio>
		</li>
		<li class="rounded p-2 hover:bg-gray-100 dark:hover:bg-gray-600">
			<Radio name="group2" bind:group={group2} value={2}>Retired</Radio>
		</li>
		<li class="rounded p-2 hover:bg-gray-100 dark:hover:bg-gray-600">
			<Radio name="group2" bind:group={group2} value={3}>Under Construction</Radio>
		</li>
	</Dropdown>
</div>
