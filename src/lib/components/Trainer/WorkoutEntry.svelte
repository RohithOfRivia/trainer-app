<script lang="ts">
    import AvatarPlaceholder from "$lib/components/AvatarPlaceholder.svelte";
    import ActionButton from "$lib/components/ActionButton.svelte";
    import Icon from "$lib/components/Icon.svelte";
    import TextInput from "../TextInput.svelte";

    let workoutData = $state({
        clientName: "",
        date: new Date().toISOString().split("T")[0],
        notes: "",
        exercises: [{ id: 1, name: "", sets: 1, reps: 0, weight: 0 }],
    });

    let openExercises = $state<Set<number>>(new Set([1]));

    function toggleExercise(id: number) {
        const next = new Set(openExercises);
        next.has(id) ? next.delete(id) : next.add(id);
        openExercises = next;
    }

    function expandAll() {
        openExercises = new Set(workoutData.exercises.map((e) => e.id));
    }

    function collapseAll() {
        openExercises = new Set();
    }

    function addExercise() {
        const id = Date.now();
        workoutData.exercises.push({
            id,
            name: "",
            sets: 1,
            reps: 0,
            weight: 0,
        });
        openExercises = new Set([...openExercises, id]);
    }

    function removeExercise(id: number) {
        workoutData.exercises = workoutData.exercises.filter(
            (ex) => ex.id !== id,
        );
        const next = new Set(openExercises);
        next.delete(id);
        openExercises = next;
    }

    function saveWorkout() {
        console.log("Saving workout:", workoutData);
    }
</script>

<div
    class="card bg-base-200 shadow-lg w-full max-w-2xl mx-auto rounded-3xl overflow-hidden"
>
    <div class="card-body p-4 sm:p-6 gap-6">
        <!-- Header -->
        <div
            class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 border-b border-base-300 pb-4"
        >
            <div class="flex items-center gap-3">
                <AvatarPlaceholder
                    name={workoutData.clientName || "New Client"}
                    rounded="full"
                />
                <h3 class="flex items-center justify center">Client Name</h3>
            </div>
            <input
                type="date"
                class="input input-bordered input-sm"
                bind:value={workoutData.date}
            />
        </div>

        <!-- Exercises -->
        <div class="flex flex-col gap-3">
            <div class="flex justify-between items-center px-1">
                <h3 class="font-semibold text-lg">Exercises</h3>
                <div class="join gap-2">
                    <div class="join-item">
                        <ActionButton
                            className="btn-soft btn-sm btn-circle"
                            icon="keyboard_double_arrow_up"
                            color="btn-ghost"
                            onclick={collapseAll}
                        />
                    </div>
                    <div class="join-item">
                        <ActionButton
                            className="btn-soft btn-sm btn-circle"
                            icon="stat_minus_2"
                            color="btn-ghost"
                            onclick={expandAll}
                        />
                    </div>
                </div>
            </div>

            {#each workoutData.exercises as exercise, index (exercise.id)}
                {@const isOpen = openExercises.has(exercise.id)}
                <div
                    class="bg-base-100 rounded-2xl shadow-sm border border-base-300 overflow-hidden"
                >
                    <button
                        class="w-full flex items-center gap-2 px-4 py-3 font-medium hover:bg-base-200 transition-colors text-left"
                        onclick={() => toggleExercise(exercise.id)}
                    >
                        <span class="badge badge-primary badge-sm"
                            >{index + 1}</span
                        >
                        <span class="flex-1"
                            >{exercise.name || "New Exercise"}</span
                        >
                        <Icon
                            name="expand_more"
                            className="text-sm opacity-50 transition-transform duration-200 {isOpen
                                ? 'rotate-180'
                                : ''}"
                        />
                    </button>

                    {#if isOpen}
                        <div
                            class="px-4 pb-4 flex flex-col gap-4 border-t border-base-300"
                        >
                            <div class="form-control w-full pt-3">
                                <label class="label"
                                    ><span class="label-text"
                                        >Exercise Name</span
                                    ></label
                                >
                                <input
                                    type="text"
                                    placeholder="e.g. Barbell Squat"
                                    class="input input-bordered w-full"
                                    bind:value={exercise.name}
                                />
                            </div>

                            <div class="grid grid-cols-3 gap-2 sm:gap-4">
                                <div class="form-control">
                                    <label class="label"
                                        ><span class="label-text text-xs"
                                            >Sets</span
                                        ></label
                                    >
                                    <input
                                        type="number"
                                        class="input input-bordered text-center"
                                        bind:value={exercise.sets}
                                        min="1"
                                    />
                                </div>
                                <div class="form-control">
                                    <label class="label"
                                        ><span class="label-text text-xs"
                                            >Reps</span
                                        ></label
                                    >
                                    <input
                                        type="number"
                                        class="input input-bordered text-center"
                                        bind:value={exercise.reps}
                                        min="0"
                                    />
                                </div>
                                <div class="form-control">
                                    <label class="label"
                                        ><span class="label-text text-xs"
                                            >Weight (lbs)</span
                                        ></label
                                    >
                                    <input
                                        type="number"
                                        class="input input-bordered text-center"
                                        bind:value={exercise.weight}
                                        min="0"
                                    />
                                </div>
                            </div>

                            <div class="flex justify-end pt-1">
                                <button
                                    class="btn btn-soft btn-circle btn-error"
                                    onclick={() => removeExercise(exercise.id)}
                                >
                                    <Icon name="delete" className="text-sm" />
                                </button>
                            </div>
                        </div>
                    {/if}
                </div>
            {/each}

            <button
                class="btn btn-outline btn-primary border-dashed rounded-2xl mt-2"
                onclick={addExercise}
            >
                <Icon name="add" /> Add Exercise
            </button>
        </div>

        <!-- Notes -->
        <label class="form-control w-full relative mt-2">
            <Icon
                name="edit_note"
                className="absolute top-3 left-3 opacity-40"
            />
            <textarea
                class="textarea textarea-bordered h-24 rounded-2xl pl-10 pt-3"
                placeholder="Session notes, injuries, overall feeling..."
                bind:value={workoutData.notes}
            ></textarea>
        </label>

        <!-- Footer -->
        <div class="card-actions justify-end border-t border-base-300 pt-4">
            <ActionButton
                label="Save Workout"
                icon="save"
                color="btn-primary"
                onclick={saveWorkout}
                fullWidth={true}
            />
        </div>
    </div>
</div>
