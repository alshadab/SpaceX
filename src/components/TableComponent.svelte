<script>
	import {
		Button,
		Modal,
		Table,
		TableBody,
		TableBodyCell,
		TableBodyRow,
		TableHead,
		TableHeadCell
	} from 'flowbite-svelte';
	import { LinkOutline } from 'flowbite-svelte-icons';
	import { goto } from '$app/navigation';
	import { writable } from 'svelte/store';

	import { onMount } from 'svelte';
	import TableLoader from '../utils/TableLoader.svelte';
	let data;
	let promise = fetch('https://api.spacexdata.com/v3/landpads').then((res) => res.json());

	import { Badge } from 'flowbite-svelte';
	import SuccesRating from '../utils/succesRating.svelte';

	function capitalizeFirstLetter(str) {
		return str.charAt(0).toUpperCase() + str.slice(1);
	}

	function navigateToAbout(url) {
		window.location = url;
	}
	// export let group = '';
	let showModal = $state(false);
	let details = $state('');
	let title = $state('');

	function openModal(item) {
		showModal = true;
		details = item.details;
		title = item.full_name;
	}
</script>

<div class="mt-10 w-full">
	{#await promise}
		<TableLoader />
	{:then data}
		<Table>
			<TableHead class="border-1 border">
				<TableHeadCell>Full Name</TableHeadCell>
				<TableHeadCell>Location name</TableHeadCell>
				<TableHeadCell>Region</TableHeadCell>
				<TableHeadCell>Details</TableHeadCell>
				<TableHeadCell>Success Rate</TableHeadCell>
				<TableHeadCell>Wikipedia Link</TableHeadCell>
				<TableHeadCell>Status</TableHeadCell>
			</TableHead>
			{#each data as item}
				<TableBody tableBodyClass="divide-y">
					<TableBodyRow class="border-1 border">
						<TableBodyCell>{item.full_name}</TableBodyCell>
						<TableBodyCell>{item.location.name}</TableBodyCell>
						<TableBodyCell>{item.location.region}</TableBodyCell>
						<TableBodyCell
							><Badge
								class="cursor-pointer hover:bg-gray-200"
								onclick={() => openModal(item)}
								color="dark">View Details</Badge
							></TableBodyCell
						>
						<TableBodyCell
							><SuccesRating
								attempted_landings={item.attempted_landings}
								successful_landings={item.successful_landings}
							/></TableBodyCell
						>
						<TableBodyCell>
							<LinkOutline
								class="cursor-pointer text-blue-700"
								onclick={() => navigateToAbout(item.wikipedia)}
							/>
						</TableBodyCell>
						<TableBodyCell>
							{#if item.status === 'active'}
								<Badge color="green">{capitalizeFirstLetter(item.status)}</Badge>
							{:else if item.status === 'retired'}
								<Badge color="red">{capitalizeFirstLetter(item.status)}</Badge>
							{:else}
								<Badge color="blue">{capitalizeFirstLetter(item.status)}</Badge>
							{/if}
						</TableBodyCell>
					</TableBodyRow>
				</TableBody>
			{/each}
		</Table>
	{:catch error}
		<p>Error: {error.message}</p>
	{/await}

	<Modal title="Details - {title}" bind:open={showModal} autoclose>
		<p class="text-base leading-relaxed text-gray-500 dark:text-gray-400">
			{details}
		</p>
	</Modal>
</div>
