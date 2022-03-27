<script>
    import Departure from '../Departure/Departure.svelte';

    const headers = [
        {key: 'Linja', class: 'line'},
        {key: 'Määränpää', class: 'destination'},
        {key: 'Lait./Pys.', class: 'platform'},
        {key: 'Aika/min', class: 'time'},
    ]

    let stops = [
        {name: 'Kukkumäki', ids: ['HSL:2433212']},
        {name: 'Matinkylä', ids: [`"HSL:2314208"`, `"HSL:2314212"`, `"HSL:2314216"`, `"HSL:2311210"`, `"HSL:2314211"`]}]
</script>

<style>
.departures {
    display: flex;
    padding: 10px 25px;
    font-family: 'Montserrat', sans-serif;
    font-size: 26px;
    color: white;
    text-align: left;
    justify-content: center;
}

.departures::after {
    position: absolute;
    content: '';
    background: #fff;
    width: 6px;
    border-radius: 5px 5px 5px 5px;
    height: calc(100% - 490px);
}

.column-header-table {
    display: flex;
    border-collapse: collapse;
    justify-content: space-between;
    border-bottom: 5px solid #fff;
}

.right {
    width: 100%;
    padding-left: 25px;
}

.left {
    width:100%;
    padding-right: 25px;
}

.time {
    text-align: right;
}

.container {
    display: flex;
    justify-content: space-around;
}
</style>


<div class="departures">
    <div class="left">
        <div class="column-header-table">
            {#each headers as header}
                <span class={header.class}>{header.key}</span>
            {/each}
        </div>
    </div>
    
    <div class="right">
        <div class="column-header-table">
            {#each headers as header}
                <span class={header.class}>{header.key}</span>
            {/each}
        </div>
    </div>
</div>
<div class="container">
    <Departure stop={stops[0].ids}/>
    <Departure stop={stops[1].ids}/>
</div>