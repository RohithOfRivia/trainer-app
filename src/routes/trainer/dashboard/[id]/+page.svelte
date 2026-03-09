<script>
    import { goto } from "$app/navigation";
    import AvatarPlaceholder from "$lib/components/AvatarPlaceholder.svelte";
    import Badge from "$lib/components/Badge.svelte";
    import Icon from "$lib/components/Icon.svelte";
    import DateInput from "$lib/components/Input/DateInput.svelte";
    import MenuTabs from "$lib/components/MenuTabs.svelte";
    import MobileMenuTabs from "$lib/components/MobileMenuTabs.svelte";
    import TextInput from "$lib/components/TextInput.svelte";
    import MealBadge from "$lib/components/Trainer/Clients/MealBadge.svelte";

    let activeTab = $state("past-meals");

    const pastMealsData = [
        {
            weekLabel: "May 26 - Jun 1, 2025",
            days: [
                {
                    date: "28 May 2025",
                    totalKcal: 1680,
                    totalP: "134g",
                    totalF: "38g",
                    meals: [
                        {
                            type: "Breakfast",
                            style: "bg-warning/20 text-warning-content border-none",
                            name: "Oats with banana & protein powder",
                            kcal: 480,
                            p: "38g",
                            f: "8g",
                        },
                        {
                            type: "Lunch",
                            style: "bg-success/20 text-success-content border-none",
                            name: "Chicken rice bowl with broccoli",
                            kcal: 620,
                            p: "52g",
                            f: "12g",
                        },
                        {
                            type: "Dinner",
                            style: "bg-primary/20 text-primary-content border-none",
                            name: "Salmon with sweet potato & greens",
                            kcal: 580,
                            p: "44g",
                            f: "18g",
                        },
                    ],
                },
                {
                    date: "27 May 2025",
                    totalKcal: 1140,
                    totalP: "94g",
                    totalF: "36g",
                    meals: [
                        {
                            type: "Breakfast",
                            style: "bg-warning/20 text-warning-content border-none",
                            name: "Greek yogurt with berries",
                            kcal: 320,
                            p: "24g",
                            f: "6g",
                        },
                        {
                            type: "Lunch",
                            style: "bg-success/20 text-success-content border-none",
                            name: "Turkey wrap with avocado",
                            kcal: 540,
                            p: "40g",
                            f: "20g",
                        },
                        {
                            type: "Snack",
                            style: "bg-secondary/20 text-secondary-content border-none",
                            name: "Protein shake with almonds",
                            kcal: 280,
                            p: "30g",
                            f: "10g",
                        },
                    ],
                },
                {
                    date: "26 May 2025",
                    totalKcal: 1100,
                    totalP: "76g",
                    totalF: "38g",
                    meals: [
                        {
                            type: "Breakfast",
                            style: "bg-warning/20 text-warning-content border-none",
                            name: "Scrambled eggs with toast",
                            kcal: 420,
                            p: "28g",
                            f: "16g",
                        },
                        {
                            type: "Dinner",
                            style: "bg-primary/20 text-primary-content border-none",
                            name: "Beef stir-fry with rice",
                            kcal: 680,
                            p: "48g",
                            f: "22g",
                        },
                    ],
                },
            ],
        },
        {
            weekLabel: "May 19 - May 25, 2025",
            days: [
                {
                    date: "25 May 2025",
                    totalKcal: 1500,
                    totalP: "120g",
                    totalF: "45g",
                    meals: [
                        {
                            type: "Breakfast",
                            style: "bg-warning/20 text-warning-content border-none",
                            name: "Protein Pancakes",
                            kcal: 500,
                            p: "40g",
                            f: "15g",
                        },
                        {
                            type: "Lunch",
                            style: "bg-success/20 text-success-content border-none",
                            name: "Tuna Salad Sandwich",
                            kcal: 450,
                            p: "35g",
                            f: "10g",
                        },
                        {
                            type: "Dinner",
                            style: "bg-primary/20 text-primary-content border-none",
                            name: "Chicken Fajitas",
                            kcal: 550,
                            p: "45g",
                            f: "20g",
                        },
                    ],
                },
            ],
        },
    ];

    let startDate = $state("");
    let endDate = $state("");
</script>

{#snippet workoutsContent()}
    <div class="flex flex-col gap-2">Content: Recent Workouts</div>
{/snippet}

{#snippet mealsContent()}
    <div class="flex flex-col gap-6">
        <div class="flex flex-col gap-4 bg-base-300 py-3 px-4 rounded-box">
            <div class="flex flex-wrap gap-4 w-full">
                <DateInput
                    placeholder="Start Date"
                    bind:value={startDate}
                    style="flex-1 min-w-[140px] rounded-box"
                />
                <DateInput
                    placeholder="End Date"
                    bind:value={endDate}
                    style="flex-1 min-w-[140px] rounded-box"
                />
            </div>
            <div class="w-full">
                <TextInput placeholder="Search..." style="w-full rounded-box" />
            </div>
        </div>
        <div class="flex flex-col gap-4">
            {#each pastMealsData as week}
                <div class="collapse collapse-arrow bg-base-200 rounded-box">
                    <input type="checkbox" checked />

                    <div class="collapse-title font-semibold text-lg">
                        {week.weekLabel}
                    </div>

                    <div class="collapse-content flex flex-col gap-8 pt-2">
                        {#each week.days as day}
                            <div class="flex flex-col gap-3">
                                <div
                                    class="flex justify-between items-center px-2"
                                >
                                    <span class="font-bold text-base-content"
                                        >{day.date}</span
                                    >
                                    <div
                                        class="flex gap-4 text-sm font-medium text-base-content/60"
                                    >
                                        <span>{day.totalKcal} kcal</span>
                                        <span>{day.totalP} P</span>
                                        <span>{day.totalF} F</span>
                                    </div>
                                </div>

                                <div class="flex flex-col gap-2">
                                    {#each day.meals as meal}
                                        <div
                                            class="flex flex-col sm:flex-row justify-between sm:items-center bg-base-100 p-4 rounded-xl gap-3"
                                        >
                                            <div
                                                class="flex items-center gap-4"
                                            >
                                                <div class="w-24 shrink-0">
                                                    <MealBadge
                                                        label={meal.type}
                                                    />
                                                </div>
                                                <span
                                                    class="font-medium text-base-content"
                                                    >{meal.name}</span
                                                >
                                            </div>
                                            <div
                                                class="flex gap-4 text-sm font-semibold sm:w-64 justify-between sm:justify-end"
                                            >
                                                <span
                                                    class="text-base-content w-20 text-right"
                                                    >{meal.kcal} kcal</span
                                                >
                                                <span
                                                    class="text-success w-12 text-right"
                                                    >{meal.p} P</span
                                                >
                                                <span
                                                    class="text-info w-12 text-right"
                                                    >{meal.f} F</span
                                                >
                                            </div>
                                        </div>
                                    {/each}
                                </div>
                            </div>
                        {/each}
                    </div>
                </div>
            {/each}
        </div>
    </div>
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
                    id: "past-meals",
                    label: "Recent Meals",
                    icon: "restaurant",
                    content: mealsContent,
                },
                {
                    id: "past-workouts",
                    label: "Recent Workouts",
                    icon: "trending_up",
                    content: workoutsContent,
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
