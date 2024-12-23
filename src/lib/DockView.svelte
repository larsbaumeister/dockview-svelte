<script lang="ts">
    import { createDockview, type GroupPanelPartInitParameters, type IContentRenderer } from "dockview-core";
    import { mount, onMount, type Component } from "svelte";
    import HelloPanel from "./panels/HelloPanel.svelte";
    import InfoPanel from "./panels/InfoPanel.svelte";

    let dockviewParent: HTMLDivElement;
    let panelsParent: HTMLDivElement;

    let components: { [key: string]: Component<any> } = {
        "hello": HelloPanel,
        "info": InfoPanel,
    }
    onMount(() => {
        const api = createDockview(dockviewParent, {
            createComponent: (option) => {
                return new Panel(option.id, components[option.name]);
            },
        });
        api.addPanel({ id: "hello1", title: "Hello 1", component: "hello", params: { name: "1" } });
        api.addPanel({ id: "hello2", title: "Hello 2", component: "hello", params: { name: "2" } });
        api.addPanel({ id: "info1", title: "Info 1", component: "info", params: { name: "1" } });
        api.addPanel({ id: "info2", title: "Info 2", component: "info", params: { name: "2" } });
    });

    class Panel implements IContentRenderer {
        private readonly _element: HTMLElement;
        private readonly _component: Component<any>;

        get element(): HTMLElement {
            return this._element;
        }

        constructor(id: string, component: Component<any>) {
            this._component = component;

            let element = document.createElement('div');
            element.id = id;
            panelsParent.appendChild(element);
            this._element = element;
        }

        init(parameters: GroupPanelPartInitParameters): void {
            mount(this._component, { target: this._element, props: parameters });
        }
    }

</script>

<div class="w-full h-full dockview-theme-replit" bind:this={dockviewParent}></div>
<div bind:this={panelsParent}></div>