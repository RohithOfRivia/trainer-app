<script>
    import DateInput from "$lib/components/Input/DateInput.svelte";
    import SearchInput from "$lib/components/SearchInput.svelte";
    import MealBadge from "$lib/components/Trainer/Clients/MealBadge.svelte";

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

<div class="flex flex-col gap-2">
    <div class="flex flex-col md:flex-row gap-4 pb-2">
        <div class="flex flex-wrap gap-4 w-full md:w-fit">
            <DateInput
                placeholder="Start Date"
                bind:value={startDate}
                style="flex-1 min-w-[140px] rounded-box w-full sm:w-fit"
            />
            <DateInput
                placeholder="End Date"
                bind:value={endDate}
                style="flex-1 min-w-[140px] rounded-box w-full sm:w-fit"
            />
        </div>
        <SearchInput placeholder="Search for Meal..." className="rounded-box w-full sm:w-1/2 md:w-fit"/>
    </div>

    <div class="flex flex-col gap-4">
        {#each pastMealsData as week}
            <div class="collapse collapse-arrow bg-base-300 border border-base-content/10 rounded-box">
                <input type="checkbox" checked />
                <div class="collapse-title font-semibold text-lg">{week.weekLabel}</div>
                <div class="collapse-content flex flex-col gap-8 pt-2">
                    {#each week.days as day}
                        <div class="flex flex-col gap-3">
                            <div class="flex justify-between items-center px-2">
                                <span class="font-bold text-base-content">{day.date}</span>
                                <div class="flex gap-4 text-sm font-medium text-base-content/60">
                                    <span>{day.totalKcal} kcal</span>
                                    <span>{day.totalP} P</span>
                                    <span>{day.totalF} F</span>
                                </div>
                            </div>
                            <div class="flex flex-col gap-2">
                                {#each day.meals as meal}
                                    <div class="flex flex-col sm:flex-row justify-between sm:items-center bg-base-100 p-4 rounded-xl gap-3">
                                        <div class="flex items-center gap-4">
                                            <div class="w-24 shrink-0">
                                                <MealBadge label={meal.type} />
                                            </div>
                                            <span class="font-medium text-base-content">{meal.name}</span>
                                        </div>
                                        <div class="flex gap-4 text-sm font-semibold sm:w-64 justify-between sm:justify-end">
                                            <span class="text-base-content w-20 text-right">{meal.kcal} kcal</span>
                                            <span class="text-success w-12 text-right">{meal.p} P</span>
                                            <span class="text-info w-12 text-right">{meal.f} F</span>
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
