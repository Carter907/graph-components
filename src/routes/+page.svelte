<script lang="ts">
  import { Graph } from "$lib/index.js";

  const adjMat: number[][] = Array.from({ length: 16 }, () =>
    Array.from({ length: 16 }, () => Math.round(Math.random())),
  );
</script>

<Graph {adjMat} />
