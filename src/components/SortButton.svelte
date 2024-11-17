<script>
	import { ButtonGroup, Button, Dropdown, DropdownItem, Radio } from 'flowbite-svelte';
	import {
		AdjustmentsVerticalOutline,
		ChevronDownOutline,
		GridSolid,
		ListOutline
	} from 'flowbite-svelte-icons';
	import { get } from 'svelte/store';
	import { sharedState,updatedData } from '../shared/store';


	let group =$state( "null");
	let type = $state(false)

	sharedState.subscribe((value) => {
		if(value === "null"){
			group = null
		}else{
			group = value;
		}
	});

	updatedData.subscribe((value) => {
		type = value;
	});


	function handleChange(event) {
		group = event.target.value;
		sharedState.set(group); 
	}

	function handleChangeType(data) {
		type =data
		updatedData.set(type); 
	}
</script>

<div class="flex w-[66%] items-center justify-between">
	<ButtonGroup>
		<Button   checked={!type} onclick={() =>handleChangeType(false) }  outline >
			<ListOutline class="me-2 h-4 w-4 " />
		</Button>
		<Button checked={type}  onclick={() =>handleChangeType(true) } outline >
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
				name="all"
				class="focus:text-indigo-600 focus:ring-2 focus:ring-indigo-600"
				bind:group
				on:change={handleChange}
				value="null">All</Radio
			>
		</li>
		<li class="rounded p-2 hover:bg-gray-100 dark:hover:bg-gray-600">
			<Radio
				name="group"
				class="focus:text-indigo-600 focus:ring-2 focus:ring-indigo-600"
				bind:group
				on:change={handleChange}
				value="active">Active</Radio
			>
		</li>
		<li class="rounded p-2 hover:bg-gray-100 dark:hover:bg-gray-600">
			<Radio name="group" bind:group on:change={handleChange} value="retired">Retired</Radio>
		</li>
		<li class="rounded p-2 hover:bg-gray-100 dark:hover:bg-gray-600">
			<Radio name="group" bind:group on:change={handleChange} value="under construction"
				>Under Construction</Radio
			>
		</li>
	</Dropdown>
</div>
