
<script>
    export let width, margin, label, yoffset, xoffset;
    
    export let labelfory = false;
  
    export let textanchor = 'end';
  </script>
  
  {#if width}
    <g class="labels-x-y text-sm">
      
      {#if labelfory}
        <text
          fill="#F2F3F4"
          x={+margin.left + xoffset}
          y={-margin.top + 80 + yoffset}
          text-anchor={textanchor}>{label}</text>
      {/if}
    </g>
  {/if}