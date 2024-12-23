<script lang="ts">
    import type { GroupPanelPartInitParameters } from "dockview-core";

    const params: GroupPanelPartInitParameters = $props();
</script>

<h1>Hello { params.params.name }</h1>