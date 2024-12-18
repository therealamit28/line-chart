<script>
    import { scaleTime, scaleLinear } from 'd3-scale';
    import { extent, max,mean} from 'd3-array';
    import { line, curveBasis } from 'd3-shape';
    import natural from '../lib/natural.js';
    import Axis from '../lib/components/Axis.svelte';
    import { draw } from 'svelte/transition';
    import Labels from '../lib/components/Labels.svelte';

    let xScale, yScale;
    let data = natural;
    let width;
    const height = 500;
    const margin = { top: 20, right: 0, bottom: 30, left: 40 };

    $: if (data && width) {
        xScale = scaleTime()
            .domain(extent(data, (d) => new Date(d.date)))
            .range([margin.left, width - margin.right]);

        yScale = scaleLinear()
            .domain([0, max(data, (d) => +d.price + 4)])
            .range([height - margin.bottom, margin.top]);
    }

    $: lineGenerator = line()
        .x((d) => xScale(new Date(d.date)))
        .y((d) => yScale(+d.price))
        .curve(curveBasis);

        $: meanValue = mean(data, d => +d.price);
        
        
        
      
            

   
</script>

<div class="wrapper" bind:clientWidth={width} style="width: 100%; max-width: 1000px; padding-top: 70px;">
    
    
    
    {#if data && width}
    <svg {width} {height}>
      
        <Axis
            {width}
            {height}
            {margin}
            tick_number={width > 380 ? 7 : 4}
            scale={xScale}
            position="bottom" />
        <Axis {width} {height} {margin} scale={yScale} position="left" />
        <Labels
            labelfory={true}
            {width}
            {height}
            {margin}
            tick_number={10}
            yoffset={-10}
            xoffset={300}
            label={'$ price per million British thermal units ↑'} />
        <path
            in:draw={{ duration: 2000 }}
            shape-rendering="crispEdges"
            d={lineGenerator(data)}
            stroke="#F2F3F4"
            stroke-width={1.5}
            stroke-linecap="round"
            fill="none" />

           <line 
            x1={margin.left}
            x2={width-margin.right}
             y1={yScale(meanValue)}
             y2={yScale(meanValue)}
             stroke="#FF0800"
             stroke-width={1.5}
             stroke-dasharray="5,5"
             in:draw={{ duration: 2500 }} 
             />
    
             
    </svg>
    {/if}
</div>

<style>
    :global(body) {
        background-color: #2A3439;
        margin: 0;
        padding: 0;
        height: 100vh;
    }
</style>
