<script lang="ts">
    import { fly, slide } from "svelte/transition";
    import AvatarPlaceholder from "$lib/components/AvatarPlaceholder.svelte";
    import ActionButton from "$lib/components/ActionButton.svelte";
    import Icon from "$lib/components/Icon.svelte";
    import TimeInput from "../Input/TimeInput.svelte";

    interface ExerciseSet {
        id: number;
        weight: number | string;
        reps: number | string;
    }

    interface Exercise {
        id: number;
        name: string;
        sets: ExerciseSet[];
    }

    const SUGGESTIONS = [
        "Barbell Squat", "Bench Press", "Deadlift", "Overhead Press", "Barbell Row", "Pull-ups", "Lunges", "Lat Pulldown", "Leg Press", "Dumbbell Curl", "Tricep Extension", "Cable Fly", "Hip Thrust", "Romanian Deadlift", "Incline Bench Press",
    ];

    let workout = $state({
        clientName: "",
        date: new Date().toISOString().split("T")[0],
        startTime: new Date().toTimeString().slice(0, 5),
        endTime: "",
        notes: "",
        exercises: [{ id: 1, name: "", sets: [{ id: Date.now(), weight: "", reps: "" }] }],
    });

    let editingName = $state(false);
    let showNotes = $state(false);
    let openExercises = $state<Set<number>>(new Set([1]));
    let activeAutocomplete = $state<number | null>(null);
    let autocompleteIndex = $state(-1);

    // Refs keyed by set ID
    const nameInputs = {} as Record<number, HTMLInputElement>;
    const weightInputs = {} as Record<number, HTMLInputElement>;
    const repsInputs = {} as Record<number, HTMLInputElement>;

    let exerciseCards = {} as Record<number, HTMLElement>;

    function focusWeight(setId: number) {
        setTimeout(() => {
            weightInputs[setId]?.focus();
            weightInputs[setId]?.select();
        }, 10);
    }

    function focusReps(setId: number) {
        setTimeout(() => {
            repsInputs[setId]?.focus();
            repsInputs[setId]?.select();
        }, 10);
    }

    function handleNameKeydown(e: KeyboardEvent, exercise: Exercise) {
        const options = autocompleteOptions(exercise.name);
        if (activeAutocomplete === exercise.id && options.length > 0) {
            if (e.key === "ArrowDown") {
                e.preventDefault();
                autocompleteIndex = Math.min(options.length - 1, autocompleteIndex + 1);
                return;
            }
            if (e.key === "ArrowUp") {
                e.preventDefault();
                autocompleteIndex = Math.max(0, autocompleteIndex - 1);
                return;
            }
            if (e.key === "Escape") {
                activeAutocomplete = null;
                autocompleteIndex = -1;
                return;
            }
            if (e.key === "Enter") {
                e.preventDefault();
                if (autocompleteIndex >= 0) exercise.name = options[autocompleteIndex];
                activeAutocomplete = null;
                autocompleteIndex = -1;
                focusWeight(exercise.sets[0].id);
                return;
            }
        }
        if (e.key === "Enter") {
            e.preventDefault();
            activeAutocomplete = null;
            focusWeight(exercise.sets[0].id);
        }
    }

    function handleWeightKeydown(e: KeyboardEvent, setId: number) {
        if (e.key === "Enter") {
            e.preventDefault();
            focusReps(setId);
        }
    }

    function handleRepsKeydown(e: KeyboardEvent, exercise: Exercise, setIndex: number) {
        if (e.key === "Enter") {
            e.preventDefault();
            const nextSet = exercise.sets[setIndex + 1];
            if (nextSet) {
                focusWeight(nextSet.id);
            }
            // If no next set, do nothing — user can manually add a set
        }
    }

    let dateInfo = $derived.by(() => {
        const target = new Date(workout.date + "T00:00:00");
        const today = new Date();
        const yesterday = new Date(today);
        yesterday.setDate(today.getDate() - 1);

        let label = target.toLocaleDateString("en-US", { weekday: "long" });
        if (target.toDateString() === today.toDateString()) label = "Today";
        if (target.toDateString() === yesterday.toDateString()) label = "Yesterday";

        return {
            label,
            display: target.toLocaleDateString("en-US", {
                month: "short",
                day: "numeric",
                year: "numeric",
            }),
        };
    });

    let autocompleteOptions = $derived((name: string) => {
        if (!name.trim()) return [];
        const lower = name.toLowerCase();
        return SUGGESTIONS.filter(
            (s) => s.toLowerCase().includes(lower) && s.toLowerCase() !== lower,
        ).slice(0, 5);
    });

    function toggleExercise(id: number) {
        const next = new Set(openExercises);
        next.has(id) ? next.delete(id) : next.add(id);
        openExercises = next;
    }

    function addExercise() {
        const id = Date.now();
        workout.exercises.push({ id, name: "", sets: [{ id: Date.now(), weight: "", reps: "" }] });
        openExercises = new Set([...openExercises, id]);

        setTimeout(() => {
            exerciseCards[id]?.scrollIntoView({ behavior: "smooth", block: "center" });
            nameInputs[id]?.focus();
        }, 80);
    }

    function removeExercise(id: number) {
        if (workout.exercises.length <= 1) return;
        workout.exercises = workout.exercises.filter((ex) => ex.id !== id);
        openExercises.delete(id);
    }

    function addSet(exerciseIndex: number) {
        const exercise = workout.exercises[exerciseIndex];
        const lastSet = exercise.sets[exercise.sets.length - 1];
        const newSetId = Date.now();

        exercise.sets.push({
            id: newSetId,
            weight: lastSet ? lastSet.weight : "",
            reps: lastSet ? lastSet.reps : "",
        });

        // Focus new set's weight after DOM update
        setTimeout(() => {
            weightInputs[newSetId]?.focus();
            weightInputs[newSetId]?.select();
        }, 50);
    }

    function removeSet(exerciseIndex: number, setId: number) {
        const exercise = workout.exercises[exerciseIndex];
        if (exercise.sets.length > 1) {
            exercise.sets = exercise.sets.filter((s) => s.id !== setId);
        }
    }
</script>

<div class="w-full max-w-[580px] mx-auto px-4 py-6 sm:py-10 flex flex-col gap-5">
    <header class="flex items-start justify-between gap-4">
        <div class="flex items-center gap-3 flex-1 min-w-0">
            <AvatarPlaceholder
                name={workout.clientName || "New Client"}
                rounded="full"
            />
            <div class="flex flex-col min-w-0">
                {#if editingName}
                    <input
                        class="bg-base-content/5 border border-primary/40 rounded-lg text-base-content px-3 py-1 text-sm outline-none w-44 font-medium"
                        placeholder="Client name…"
                        autofocus
                        bind:value={workout.clientName}
                        onblur={() => (editingName = false)}
                        onkeydown={(e) => e.key === "Enter" && (editingName = false)}
                    />
                {:else}
                    <h1 class="text-2xl font-bold leading-tight m-0 truncate">
                        {workout.clientName || "Client Name"}
                    </h1>
                    <div class="flex items-center gap-1.5 text-xs mt-1">
                        <button
                            class="text-primary font-medium hover:opacity-80 transition-opacity"
                            onclick={() => (editingName = true)}
                        >{dateInfo.label}</button>
                        <span class="text-base-content/50">·</span>
                        <label class="relative flex items-center gap-1 cursor-pointer group">
                            <span class="text-base-content/60 group-hover:text-base-content transition-colors">{dateInfo.display}</span>
                            <span class="material-symbols-outlined" style="font-size: 12px;">edit</span>
                            <input
                                type="date"
                                class="absolute inset-0 opacity-0 cursor-pointer w-full"
                                bind:value={workout.date}
                            />
                        </label>
                    </div>
                    
                {/if}
            </div>
            <div class="flex items-center justify-center gap-2 flex-col md:flex-row">
            <TimeInput bind:value={workout.startTime} style="input-sm w-25" />
            <TimeInput bind:value={workout.endTime} style="input-sm w-25" />
        </div>
        </div>
    </header>

    <div class="h-px w-full bg-base-content/10"></div>

    <div class="flex flex-col gap-3">
        {#each workout.exercises as exercise, index (exercise.id)}
            {@const isOpen = openExercises.has(exercise.id)}
            <div
                bind:this={exerciseCards[exercise.id]}
                in:fly={{ y: -10, duration: 150 }}
                class="bg-base-content/5 border border-base-content/10 rounded-2xl overflow-visible transition-colors hover:border-base-content/20"
            >
                <button
                    class="w-full flex items-center gap-3 px-4 py-3.5 text-left select-none"
                    onclick={() => toggleExercise(exercise.id)}
                >
                    <span class="shrink-0 w-6 h-6 rounded-md bg-base-content/10 text-base-content/60 text-xs font-semibold flex items-center justify-center">
                        {index + 1}
                    </span>
                    <span class="flex-1 text-sm font-medium text-base-content truncate">
                        {exercise.name || "Untitled Exercise"}
                    </span>
                    {#if !isOpen && exercise.name}
                        <span class="text-xs text-base-content/50 shrink-0 hidden sm:inline">
                            {exercise.sets.length} {exercise.sets.length === 1 ? 'set' : 'sets'}
                        </span>
                    {/if}
                    <Icon
                        name="expand_more"
                        className="text-lg text-base-content/50 shrink-0 transition-transform duration-200 {isOpen ? 'rotate-180' : ''}"
                    />
                </button>

                {#if isOpen}
                    <div
                        class="px-4 pb-4 pt-2 flex flex-col gap-4 border-t border-base-content/10"
                        transition:slide={{ duration: 150 }}
                    >
                        <!-- Exercise name + autocomplete -->
                        <div class="relative">
                            <input
                                type="text"
                                class="w-full bg-base-content/5 border border-base-content/10 rounded-xl text-base-content text-sm px-4 py-2.5 outline-none focus:border-primary/50 focus:bg-base-content/10 transition-all placeholder-base-content/40"
                                placeholder="Exercise name…"
                                bind:value={exercise.name}
                                bind:this={nameInputs[exercise.id]}
                                onfocus={() => {
                                    activeAutocomplete = exercise.id;
                                    autocompleteIndex = -1;
                                }}
                                onblur={() =>
                                    setTimeout(() => {
                                        activeAutocomplete = null;
                                        autocompleteIndex = -1;
                                    }, 160)}
                                oninput={() => (autocompleteIndex = -1)}
                                onkeydown={(e) => handleNameKeydown(e, exercise)}
                            />
                            {#if activeAutocomplete === exercise.id && autocompleteOptions(exercise.name).length > 0}
                                <ul
                                    class="absolute left-0 right-0 top-[calc(100%+4px)] z-30 bg-base-200 border border-base-content/10 rounded-xl shadow-2xl py-1 overflow-hidden"
                                    in:fly={{ y: -4, duration: 120 }}
                                >
                                    {#each autocompleteOptions(exercise.name) as s, i}
                                        <li>
                                            <button
                                                class="w-full text-left px-4 py-2 text-sm flex items-center gap-2 transition-colors {autocompleteIndex === i
                                                    ? 'bg-primary/20 text-base-content font-medium'
                                                    : 'text-base-content/80 hover:bg-primary/10 hover:text-base-content'}"
                                                onmousedown={() => {
                                                    exercise.name = s;
                                                    activeAutocomplete = null;
                                                    autocompleteIndex = -1;
                                                    focusWeight(exercise.sets[0].id);
                                                }}
                                            >
                                                <Icon
                                                    name="fitness_center"
                                                    className="text-sm {autocompleteIndex === i ? 'text-primary' : 'text-primary/50'}"
                                                />
                                                {s}
                                            </button>
                                        </li>
                                    {/each}
                                </ul>
                            {/if}
                        </div>

                        <!-- Sets table -->
                        <div class="flex flex-col gap-2">
                            <div class="flex items-center gap-2 px-1 text-[10px] font-bold text-base-content/50 uppercase tracking-widest text-center">
                                <div class="w-8">Set</div>
                                <div class="flex-1">Kg</div>
                                <div class="flex-1">Reps</div>
                                <div class="w-8"></div>
                            </div>

                            {#each exercise.sets as set, setIndex (set.id)}
                                <div class="flex items-center gap-2">
                                    <div class="w-8 h-10 flex items-center justify-center rounded-lg bg-base-content/5 text-xs font-semibold text-base-content/60">
                                        {setIndex + 1}
                                    </div>
                                    <input
                                        type="number"
                                        inputmode="decimal"
                                        class="w-full flex-1 h-10 text-center bg-base-content/5 border border-base-content/10 rounded-lg text-sm outline-none focus:border-primary/50 focus:bg-base-content/10 transition-colors [appearance:textfield] [&::-webkit-inner-spin-button]:appearance-none"
                                        placeholder="-"
                                        bind:value={set.weight}
                                        bind:this={weightInputs[set.id]}
                                        onfocus={(e) => e.currentTarget.select()}
                                        onkeydown={(e) => handleWeightKeydown(e, set.id)}
                                    />
                                    <input
                                        type="number"
                                        inputmode="numeric"
                                        class="w-full flex-1 h-10 text-center bg-base-content/5 border border-base-content/10 rounded-lg text-sm outline-none focus:border-primary/50 focus:bg-base-content/10 transition-colors [appearance:textfield] [&::-webkit-inner-spin-button]:appearance-none"
                                        placeholder="-"
                                        bind:value={set.reps}
                                        bind:this={repsInputs[set.id]}
                                        onfocus={(e) => e.currentTarget.select()}
                                        onkeydown={(e) => handleRepsKeydown(e, exercise, setIndex)}
                                    />
                                    <button
                                        class="w-8 h-10 flex items-center justify-center text-base-content/40 hover:text-error transition-colors shrink-0 disabled:opacity-20 disabled:hover:text-base-content/40"
                                        onclick={() => removeSet(index, set.id)}
                                        disabled={exercise.sets.length <= 1}
                                    >
                                        <Icon name="close" className="text-lg" />
                                    </button>
                                </div>
                            {/each}

                            <button
                                class="mt-1 w-full h-10 rounded-lg border border-dashed border-base-content/20 text-xs font-medium text-base-content/60 hover:text-primary hover:border-primary/40 hover:bg-base-content/5 transition-all flex items-center justify-center gap-1"
                                onclick={() => addSet(index)}
                            >
                                <Icon name="add" className="text-base" /> Add Set
                            </button>
                        </div>

                        {#if workout.exercises.length > 1}
                            <div class="flex justify-end pt-2">
                                <ActionButton
                                    icon="delete"
                                    color="btn-error"
                                    className="btn-sm btn-circle text-base-content/50"
                                    onclick={() => removeExercise(exercise.id)}
                                />
                            </div>
                        {/if}
                    </div>
                {/if}
            </div>
        {/each}
    </div>

    <button
        class="w-full py-3.5 rounded-2xl border border-dashed border-base-content/20 flex items-center justify-center gap-2 text-sm text-base-content/60 hover:text-primary hover:bg-base-content/5 hover:border-primary/40 transition-all"
        onclick={addExercise}
    >
        <Icon name="add" className="text-lg" /> Add Exercise
    </button>

    <div class="flex flex-col">
        <button
            class="flex items-center gap-2 text-sm text-base-content/60 hover:text-base-content w-max transition-colors"
            onclick={() => (showNotes = !showNotes)}
        >
            <Icon name="edit_note" className="text-lg" /> Session Notes
            <Icon
                name="expand_more"
                className="text-sm transition-transform duration-200 {showNotes ? 'rotate-180' : ''}"
            />
            {#if workout.notes && !showNotes}
                <span class="w-1.5 h-1.5 rounded-full bg-primary ml-1"></span>
            {/if}
        </button>
        {#if showNotes}
            <div transition:slide={{ duration: 200 }} class="pt-3">
                <textarea
                    class="w-full bg-base-content/5 border border-base-content/10 rounded-xl text-base-content text-sm px-4 py-3 outline-none focus:border-primary/40 resize-y min-h-[80px] placeholder-base-content/40 transition-all"
                    placeholder="How did it feel? Any pain or PRs?"
                    bind:value={workout.notes}
                ></textarea>
            </div>
        {/if}
    </div>

    <button
        class="btn btn-primary w-full h-14 mt-2 rounded-2xl text-base shadow-[0_4px_20px_var(--fallback-p,oklch(var(--p)/0.25))] active:scale-[0.98] transition-all"
        onclick={() => console.log("Saving:", workout)}
    >
        <Icon name="check" className="text-lg" /> Save Workout
    </button>
</div>
