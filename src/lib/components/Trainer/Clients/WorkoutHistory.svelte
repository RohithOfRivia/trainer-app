<script lang="ts">
    import DateInput from "$lib/components/Input/DateInput.svelte";
    import SearchInput from "$lib/components/SearchInput.svelte";
    import Icon from "$lib/components/Icon.svelte";

    const pastWorkoutsData = [
        {
            weekLabel: "May 26 - Jun 1, 2025",
            workouts: [
                {
                    name: "Leg Day",
                    date: "28 May 2025",
                    status: "Completed",
                    exercises: [
                        {
                            name: "Barbell Squat",
                            sets: [
                                { weight: "120 kg", reps: "8 reps" },
                                { weight: "120 kg", reps: "8 reps" },
                                { weight: "120 kg", reps: "8 reps" },
                                { weight: "120 kg", reps: "8 reps" },
                            ],
                        },
                        {
                            name: "Romanian Deadlift",
                            sets: [
                                { weight: "90 kg", reps: "10 reps" },
                                { weight: "90 kg", reps: "10 reps" },
                                { weight: "90 kg", reps: "10 reps" },
                            ],
                        },
                        {
                            name: "Leg Press",
                            sets: [
                                { weight: "180 kg", reps: "12 reps" },
                                { weight: "180 kg", reps: "12 reps" },
                            ],
                        },
                    ],
                },
                {
                    name: "Push Day",
                    date: "26 May 2025",
                    status: "Completed",
                    exercises: [
                        {
                            name: "Bench Press",
                            sets: [
                                { weight: "80 kg", reps: "8 reps" },
                                { weight: "80 kg", reps: "8 reps" },
                                { weight: "80 kg", reps: "8 reps" },
                            ],
                        },
                    ],
                },
            ],
        },
        {
            weekLabel: "May 19 - May 25, 2025",
            workouts: [
                {
                    name: "Pull Day",
                    date: "24 May 2025",
                    status: "Completed",
                    exercises: [
                        {
                            name: "Pull-ups",
                            sets: [
                                { weight: "Bodyweight", reps: "10 reps" },
                                { weight: "Bodyweight", reps: "8 reps" },
                            ],
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
    <div class="flex flex-col md:flex-row gap-4 pb-4">
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
        <SearchInput
            placeholder="Search Workouts..."
            className="rounded-box w-full sm:w-1/2 md:w-fit"
        />
    </div>

    <div class="flex flex-col gap-4">
        {#each pastWorkoutsData as week}
            <div class="collapse collapse-arrow bg-base-200 rounded-box border border-base-300">
                <input type="checkbox" checked />
                <div class="collapse-title font-semibold text-lg">
                    {week.weekLabel}
                </div>
                <div class="collapse-content flex flex-col gap-4 pt-2">
                    {#each week.workouts as workout}
                        <div class="bg-base-100 rounded-box border border-base-200 overflow-hidden shadow-sm">
                            <div class="p-4 sm:px-6 flex justify-between items-center border-b border-base-200 bg-base-100">
                                <div class="flex items-center gap-4">
                                    <div class="w-10 h-10 rounded-full bg-primary/10 text-primary flex items-center justify-center shrink-0">
                                        <Icon name="fitness_center" />
                                    </div>
                                    <div class="flex flex-col">
                                        <h3 class="font-bold text-base-content m-0 leading-tight">
                                            {workout.name}
                                        </h3>
                                        <span class="text-xs font-medium text-base-content/60">
                                            {workout.date} · {workout.exercises.length} exercises
                                        </span>
                                    </div>
                                </div>
                                <span class="badge badge-soft badge-success px-3 py-1 font-medium">
                                    {workout.status}
                                </span>
                            </div>

                            <div class="p-4 sm:px-6 flex flex-col gap-8 bg-base-100">
                                {#each workout.exercises as exercise}
                                    <div class="flex flex-col">
                                        <h4 class="font-bold text-base-content mb-4 text-[15px]">
                                            {exercise.name}
                                        </h4>
                                        <div class="grid grid-cols-3 text-[11px] font-bold text-base-content/50 mb-3 px-2 tracking-wider">
                                            <span>SET</span>
                                            <span class="text-center">WEIGHT</span>
                                            <span class="text-right">REPS</span>
                                        </div>
                                        
                                        <div class="flex flex-col gap-1">
                                            {#each exercise.sets as set, i}
                                                <div class="grid grid-cols-3 text-sm px-2 py-1.5 hover:bg-base-200 rounded-lg transition-colors">
                                                    <span class="text-base-content/60 font-medium">
                                                        {i + 1}
                                                    </span>
                                                    <span class="text-center font-bold text-base-content">
                                                        {set.weight}
                                                    </span>
                                                    <span class="text-right font-bold text-base-content">
                                                        {set.reps}
                                                    </span>
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
        {/each}
    </div>
</div>
