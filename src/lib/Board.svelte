<script lang="ts">
  import { onMount } from "svelte";
  import Node from "./Node.svelte";

  let { adjMat } = $props();

  let nodes = $state<{
    [key: number]: { x: number; y: number };
  }>({
    0: { x: 0, y: 0 },
    1: { x: 0, y: 0 },
    2: { x: 0, y: 0 },
    3: { x: 0, y: 0 },
    4: { x: 0, y: 0 },
    5: { x: 0, y: 0 },
    6: { x: 0, y: 0 },
    7: { x: 0, y: 0 },
    8: { x: 0, y: 0 },
  });
  let cnts = [0, 1, 2, 3, 4, 5, 6, 7, 8];
  for (let i = cnts.length - 1; i > 0; i--) {
    // Generate a random index between 0 and i
    const j = Math.floor(Math.random() * (i + 1));

    // Swap elements at index i and j
    [cnts[i], cnts[j]] = [cnts[j], cnts[i]];
  }

  let windowDimensions = $state({
    width: 0,
    height: 0,
  });

  let boardConfig = $derived({
    squareWidth: windowDimensions.width / 3,
    squareHeight: windowDimensions.height / 3,
  });
  let nodeConfig = $derived({
    radius: Math.min(boardConfig.squareHeight, boardConfig.squareWidth) * 0.25,
  });
  onMount(() => {
    windowDimensions.width = window.innerWidth;
    windowDimensions.height = window.innerHeight;
    initNodes();
  });
  function initNodes() {
    for (let i = 0; i < 3; i++) {
      for (let j = 0; j < 3; j++) {
        const cx =
          nodeConfig.radius +
          i * boardConfig.squareWidth +
          Math.random() * (boardConfig.squareWidth * 0.5);
        const cy =
          nodeConfig.radius +
          j * boardConfig.squareHeight +
          Math.random() * (boardConfig.squareHeight * 0.5);

        const key = cnts.pop()!!;
        nodes[key].x = cx;
        nodes[key].y = cy;
      }
    }
  }
</script>

<svg viewBox="0 0 {windowDimensions.width} {windowDimensions.height}">
  {#each [0, 1, 2, 3, 4, 5, 6, 7, 8] as key}
    <Node
      r={nodeConfig.radius}
      cx={nodes[key].x}
      cy={nodes[key].y}
      value={key + 1}
    ></Node>
  {/each}

  {#each [0, 1, 2, 3, 4, 5, 6, 7, 8] as i}
    {#each [0, 1, 2, 3, 4, 5, 6, 7, 8] as j}
      {#if adjMat[i][j] == 1}
        {@const x1 = Math.floor(nodes[i]!!.x)}
        {@const y1 = Math.floor(nodes[i]!!.y)}
        {@const x2 = Math.floor(nodes[j]!!.x)}
        {@const y2 = Math.floor(nodes[j]!!.y)}
        <line {x1} {x2} {y1} {y2} stroke="blue" stroke-width="4"></line>
      {/if}
    {/each}
  {/each}
</svg>
