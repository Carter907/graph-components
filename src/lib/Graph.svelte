<script lang="ts">
  import { onMount } from "svelte";
  import Node from "./Node.svelte";
  import Edge from "./Edge.svelte";

  let {
    adjMat,
  }: {
    adjMat: number[][];
  } = $props();

  let nodes = $state<{
    [key: number]: { x: number; y: number };
  }>({});
  for (let i = 0; i < adjMat.length; i++) {
    nodes[i] = { x: 0, y: 0 };
  }
  let cnts: number[] = Array.from({ length: adjMat.length }, (_, i) => i);
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
    squareWidth: windowDimensions.width / Math.sqrt(adjMat.length),
    squareHeight: windowDimensions.height / Math.sqrt(adjMat.length),
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
    for (let i = 0; i < Math.sqrt(adjMat.length); i++) {
      for (let j = 0; j < Math.sqrt(adjMat.length); j++) {
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
  {#each Array.from({ length: adjMat.length }, (_, i) => i) as key}
    <Node
      r={nodeConfig.radius}
      cx={nodes[key].x}
      cy={nodes[key].y}
      value={key + 1}
    ></Node>
  {/each}

  {#each adjMat as row, i}
    {#each row as ele, j}
      {#if ele == 1}
        {@const node1 = {
          x: Math.floor(nodes[i]!!.x),
          y: Math.floor(nodes[i]!!.y),
        }}
        {@const node2 = {
          x: Math.floor(nodes[j]!!.x),
          y: Math.floor(nodes[j]!!.y),
        }}
        <Edge {node1} {node2}></Edge>
      {/if}
    {/each}
  {/each}
</svg>
