<script lang="ts">
    import { createSelect, melt } from "@melt-ui/svelte";
    import { fade } from "svelte/transition";
    const {
        elements: { trigger, menu, option, group, groupLabel, label },
        states: { selectedLabel, open, selected },
        helpers: { isSelected },
    } = createSelect<string>({
        forceVisible: true,
        positioning: {
            placement: "bottom",
            fitViewport: true,
            sameWidth: true,
        },
    });
    const options = ["Hostel", "Rooms", "Flats", "PG's"];

    let labelSelected = $derived(
        $selectedLabel || "Choose from hostels, rooms, ...",
    );
</script>

<main class="h-svh w-full flex justify-center items-center">
    <button
        class="flex items-center justify-between rounded-xl border border-teal-600 bg-teal-200 px-[11px] py-[10px] font-[CG] text-lg text-teal-900 hover:bg-teal-100"
        use:melt={$trigger}
        aria-label="Place Type"
    >
        {labelSelected}
        <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="size-5 shrink-0"><path d="m6 9 6 6 6-6" /></svg
        >
    </button>
    {#if $open}
        <div
            class=" flex min-h-fit flex-col gap-y-1 rounded-xl bg-teal-200 p-2 font-[CG] text-lg"
            use:melt={$menu}
            transition:fade={{ duration: 150 }}
        >
            {#each options as item}
                <div use:melt={$group(item)}>
                    <div
                        class="hover:bg-magnum-100 focus:text-magnum-700 relative flex cursor-pointer items-center justify-between rounded-lg px-3 py-1 text-neutral-800 focus:z-10
                                data-[highlighted]:rounded-xl data-[highlighted]:bg-teal-400
                                data-[disabled]:opacity-50"
                        use:melt={$option({ value: item, label: item })}
                    >
                        {item}
                        <div
                            class="check {$isSelected(item)
                                ? 'block'
                                : 'hidden'}"
                        ></div>
                    </div>
                </div>
            {/each}
        </div>
    {/if}
</main>
