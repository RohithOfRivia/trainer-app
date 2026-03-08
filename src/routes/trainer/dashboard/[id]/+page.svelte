<script>
    import { goto } from "$app/navigation";
    import AvatarPlaceholder from "$lib/components/AvatarPlaceholder.svelte";
    import Badge from "$lib/components/Badge.svelte";
    import Icon from "$lib/components/Icon.svelte";
    import MenuTabs from "$lib/components/MenuTabs.svelte";
    import MobileMenuTabs from "$lib/components/MobileMenuTabs.svelte";

    let activeTab = $state("past-workouts");

    const mealHistory = [
        {
            date: "28 May 2025",
            totalKcal: 1680,
            totalP: 134,
            totalF: 38,
            meals: [
                {
                    type: "Breakfast",
                    name: "Oats with banana & protein powder",
                    kcal: 480,
                    p: 38,
                    f: 8,
                    badgeClass: "bg-orange-500/10 text-orange-600",
                },
                {
                    type: "Lunch",
                    name: "Chicken rice bowl with broccoli",
                    kcal: 620,
                    p: 52,
                    f: 12,
                    badgeClass: "bg-success/10 text-success",
                },
                {
                    type: "Dinner",
                    name: "Salmon with sweet potato & greens",
                    kcal: 580,
                    p: 44,
                    f: 18,
                    badgeClass: "bg-primary/10 text-primary",
                },
            ],
        },
        {
            date: "27 May 2025",
            totalKcal: 1140,
            totalP: 94,
            totalF: 36,
            meals: [
                {
                    type: "Breakfast",
                    name: "Greek yogurt with berries",
                    kcal: 320,
                    p: 24,
                    f: 6,
                    badgeClass: "bg-orange-500/10 text-orange-600",
                },
                {
                    type: "Lunch",
                    name: "Turkey wrap with avocado",
                    kcal: 540,
                    p: 40,
                    f: 20,
                    badgeClass: "bg-success/10 text-success",
                },
                {
                    type: "Snack",
                    name: "Protein shake with almonds",
                    kcal: 280,
                    p: 30,
                    f: 10,
                    badgeClass: "bg-secondary/10 text-secondary",
                },
            ],
        },
    ];
</script>

{#snippet workoutsContent()}
    <div class="flex flex-col gap-2">Content: Recent Workouts</div>
{/snippet}

{#snippet mealsContent()}
    <div class="flex flex-col gap-2">Content: Recent Meals</div>
{/snippet}

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
    <div class="w-full flex flex-col gap-5">
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

            <div
                class="flex flex-row items-center justify-between
                    sm:flex-col sm:items-end w-full sm:w-auto pt-3 sm:pt-0
                    border-t border-base-content/10 sm:border-transparent shrink-0"
            >
                <span class="text-sm text-base-content/70 sm:order-last"
                    >No. of Sessions</span
                >
                <h3 class="text-2xl sm:text-4xl font-bold m-0">39</h3>
            </div>
        </div>

        <MobileMenuTabs
            bind:activeTab
            tabs={[
                {
                    id: "past-workouts",
                    label: "Recent Workouts",
                    icon: "trending_up",
                    content: workoutsContent,
                },
                {
                    id: "past-meals",
                    label: "Recent Meals",
                    icon: "restaurant",
                    content: mealsContent,
                },
            ]}
            className="block md:hidden w-full"
        />

        <MenuTabs
            tabs={[
                {
                    label: "Recent Workouts",
                    content: workoutsContent,
                },
                {
                    label: "Recent Meals",
                    content: mealsContent,
                },
            ]}
            className="hidden md:flex w-full"
        />
    </div>
</div>
