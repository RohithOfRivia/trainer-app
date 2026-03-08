<script>
    import { goto } from "$app/navigation";
    import ActionButton from "$lib/components/ActionButton.svelte";
    import AvatarPlaceholder from "$lib/components/AvatarPlaceholder.svelte";
    import Badge from "$lib/components/Badge.svelte";
    import Icon from "$lib/components/Icon.svelte";
    import MetricCard from "$lib/components/MetricCard.svelte";
    import Modal from "$lib/components/Modal.svelte";
    import ScheduleCard from "$lib/components/Trainer/Dashboard/ScheduleCard.svelte";
    import WorkoutEntry from "$lib/components/Trainer/WorkoutEntry.svelte";

    let isLogging = $state(false);
    let workout = $state({
        clientName: "",
        date: new Date().toISOString().split("T")[0],
        notes: "",
        exercises: [{ id: 1, name: "", sets: 3, reps: 10, weight: 0 }],
    });
</script>

<div class="flex flex-col gap-2 items-center">
    <!-- Go back to dashbboard -->
    <button
        class="flex w-fit self-start rounded-full items-center gap-1.5 text-base-content/60 hover:text-base-content hover:bg-base-200 shrink-0 p-2"
        onclick={() => goto("/trainer/dashboard")}
    >
        <Icon name="keyboard_backspace" className="text-base" />
        <span class="text-sm">All clients</span>
    </button>

    <!-- CLient details -->
    <div class="w-full">
    <div
        class="card bg-base-300 p-4 sm:p-5 flex flex-col sm:flex-row justify-between sm:items-center gap-4 sm:gap-10"
    >
        <div class="flex items-center gap-4 sm:gap-5">
            <AvatarPlaceholder name="Client Name" size="w-16" />

            <div class="flex flex-col gap-1">
                <h1 class="m-0 text-xl sm:text-2xl">Client Name</h1>

                <div class="flex flex-wrap items-center gap-2 sm:gap-3">
                    <Badge
                        label="Strength Training"
                        icon="fitness_center"
                        style="badge-accent"
                    />
                    <span
                        class="text-base-content/50 flex items-center gap-1 text-sm"
                    >
                        <span
                            class="material-symbols-outlined"
                            style="font-size: 14px;">calendar_today</span
                        >
                        Client since Sept 2021
                    </span>
                </div>
            </div>
        </div>

        <div class="flex flex-row sm:flex-col items-center justify-between sm:items-end w-full sm:w-auto pt-3 sm:pt-0 border-t border-base-content/10 sm:border-transparent shrink-0">
            <span class="text-sm text-base-content/70 sm:order-last">No. of Sessions</span>
            <h3 class="text-2xl sm:text-4xl font-bold m-0">39</h3>
        </div>
    </div>
</div>
</div>
