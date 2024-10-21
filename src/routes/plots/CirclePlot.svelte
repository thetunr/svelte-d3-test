<script>
  let data = [{x: 10, r: 5 },
              {x: 30, r: 10},
              {x: 50, r: 15},
              {x: 70, r: 20},
              {x: 90, r: 25}]
</script>

<svg>
  {#each data as d}
    <circle cx={d.x + "%"} cy="50%" r={d.r} fill="darkgrey" />
  {/each}
</svg>