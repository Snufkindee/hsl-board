<script>
import { operationStore, query } from '@urql/svelte'
import { onMount } from "svelte";

export let stop;

let currentTime = Math.floor(Date.now() / 1000);
let stopArray = [];

onMount(() => {
    const timeInterval = setInterval(() => {
        currentTime = Math.floor(Date.now() / 1000);
    }, 1000);

    return () => {
        clearInterval(timeInterval);
    }
})
const validateQuery = stop.length === 1 ? `stop(id: "${stop}")` : `stops(ids: [${stop}])`;
const stopQuery = operationStore(
		`
			{
				${validateQuery} {
					name
					stoptimesWithoutPatterns(startTime: ${currentTime}) {
                        stop {
                            name
                        }
                        scheduledArrival
                        realtimeArrival
                        arrivalDelay
                        scheduledDeparture
                        realtimeDeparture
                        departureDelay
                        realtime
                        realtimeState
                        serviceDay
                        headsign
                        trip {
                            route{
                                shortName
                                longName
                            }
                        }
					}
				}  
			}
		`
	);
    query(stopQuery).subscribe((query) => {
        if(query.data) {
            if(query.data.stop) {
                for(const stoptime of query.data.stop.stoptimesWithoutPatterns){
                    stopArray.push(stoptime)
                }
            } else {
                for(const stops of query.data.stops){
                    for(const stoptime of stops.stoptimesWithoutPatterns){
                            stopArray.push(stoptime)
                    }
                }
            }
            stopArray.sort((a,b) => {
                    return new Date((a.serviceDay + a.realtimeDeparture)) - new Date((b.serviceDay + b.realtimeDeparture))
                });
            stopArray = stopArray.slice(0, 5);
            console.log(stopArray)
        }
    });

</script>

<style>
  .departure {
    border-bottom: 3px dotted #fff;
    font-family: 'Montserrat', sans-serif;
    font-size: 35px;
    color: white;
    margin: 10px 25px;
    padding-bottom: 10px;
    width: 900px;
}   

.container {display: flex; flex-direction: column; width: 100%; padding-right: 50px;}
.departure { display: flex; justify-content: space-between; width: 100%;}

.line {
    flex-basis: 217px;
}

.name {
    flex-basis: 294px;
}

.stop {
    flex-basis: 348px;
}

.time {
    flex-basis: 40px;
}

</style>

<div class="container">
	{#if $stopQuery.fetching}
		Loading...
	{:else if $stopQuery.error}
		Something went wrong. {$stopQuery.error.message}
	{:else if !$stopQuery.data}
		No data.
	{:else if !$stopQuery.fetching && stopQuery.data.stop}
        {#each stopArray as stoptime}
        <div class="departure">
            <span class="line">{stoptime.trip.route.shortName}</span>
            <span class="name">{stoptime.headsign.split(" ")[0]}</span>
            <span class="stop">{$stopQuery.data.stop.name}</span>
            <span class="time">{Math.floor
                ( 
                    (
                        Math.abs
                        (
                            new Date(currentTime*1000) - new Date((stoptime.serviceDay + stoptime.realtimeDeparture)*1000)
                        )/1000
                    ) /60
                )
                }</span>
        </div>
        {/each}
    {:else}
                {#each stopArray as stoptime}
                    <div class="departure">
                        <span class="line">{stoptime.trip.route.shortName}</span>
                        <span class="name">{stoptime.headsign.split(" ")[0]}</span>
                        <span class="stop">{stoptime.stop.name}</span>
                        <span class="time">{Math.floor
                        ( 
                            (
                                Math.abs
                                (
                                    new Date((stoptime.serviceDay + stoptime.realtimeDeparture)*1000) - new Date(currentTime*1000)
                                )/1000
                            ) /60
                        )
                        }</span>
                    </div>
                {/each}
	{/if}
</div>