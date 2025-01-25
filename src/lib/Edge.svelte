<script lang="ts">
  type Node = { x: number; y: number };
  let { node1, node2 }: { node1: Node; node2: Node } = $props();
</script>

<line
  x1={node1.x}
  y1={node1.y}
  x2={node2.x}
  y2={node2.y}
  stroke="blue"
  stroke-width="4"
></line>
