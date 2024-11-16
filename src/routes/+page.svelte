<script>
	// import { derived, writable } from 'svelte/store';
	import Navbar from '../components/Navbar.svelte';
	import SortButton from '../components/SortButton.svelte';
	import TableComponent from '../components/TableComponent.svelte';
	let group = $state('active');
</script>

<Navbar />

<div class="mt-32 w-full px-24">
	<SortButton {group} />
	<TableComponent {group} />
</div>
