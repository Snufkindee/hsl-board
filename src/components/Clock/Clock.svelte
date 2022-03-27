<script>
import { onMount } from "svelte";


let time = new Date();

$: hours = time.getHours() < 10 ? '0' + time.getHours() : time.getHours();
$: minutes = time.getMinutes() < 10 ? '0' + time.getMinutes() : time.getMinutes();
$: visibility = true;

onMount(() => {
    const timeInterval = setInterval(() => {
        time = new Date();
        visibility = !visibility;
    }, 1000);

    return () => {
        clearInterval(timeInterval);
    }
})
</script>

<style>
.clock {
    width: 230px;
    float: right;
}

.clock .time {
    font-family: 'Montserrat', sans-serif;
    background: #fff;
    color: rgb(0,88,162);
    border-radius: 15px;
    box-sizing: border-box;
    font-weight: 700;
    width: 230px;
    height: 90px;
    font-size: 72px;
    line-height: 92px;
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
}

.show {
    visibility: visible;
}

.hide {
    visibility: hidden;
}
</style>


<div class="clock">
    <div class="time">
        <span>{hours}</span>
        <span class={visibility ? 'show' : 'hide'}>:</span>
        <span>{minutes}</span>
    </div>
</div>