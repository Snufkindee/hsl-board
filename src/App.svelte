<script>
	import { initClient} from '@urql/svelte'
	import Header from './components/Header/Header.svelte';
	import Departures from './components/Departures/Departures.svelte';

	initClient({
		url: 'https://api.digitransit.fi/routing/v1/routers/hsl/index/graphql'
	});
</script>

<Header />
<Departures />

<style>
</style>