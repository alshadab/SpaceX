<script>
	import Navbar from '../components/Navbar.svelte';
	import SortButton from '../components/SortButton.svelte';
	import TableComponent from '../components/TableComponent.svelte';
	let group = 'active';
</script>

<Navbar />

<div class="container mt-32 w-[73%] px-24">
	<SortButton {group} />
	<TableComponent {group} />
</div>
