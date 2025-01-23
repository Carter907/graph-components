<script>
  let { r, cx, cy, value } = $props();
  const textLength = 20;
</script>

<g>
  <circle {r} {cx} {cy} stroke="black" stroke-width="4" fill="white"> </circle>
  <text
    x={cx}
    y={cy}
    {textLength}
    dx={-textLength / 2}
    dy={textLength / 2}
    font-size="40px"
    class="font-bold">{value}</text
  >
</g>
