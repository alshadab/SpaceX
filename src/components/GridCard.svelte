<script>
	import { Badge, Card } from "flowbite-svelte";


    let {item} = $props()
    function capitalizeFirstLetter(str) {
		return str.charAt(0).toUpperCase() + str.slice(1);
	}
</script>

<Card>
    <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">{item.full_name}</h5>
    <p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">Location: {item.location.name}</p>
    <p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">Region: {item.location.region}</p>
    <p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">Status: {#if item.status === 'active'}
        <Badge color="green">{capitalizeFirstLetter(item.status)}</Badge>
    {:else if item.status === 'retired'}
        <Badge color="red">{capitalizeFirstLetter(item.status)}</Badge>
    {:else}
        <Badge color="blue">{capitalizeFirstLetter(item.status)}</Badge>
    {/if}</p>
  </Card>