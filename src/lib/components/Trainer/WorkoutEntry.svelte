<script lang="ts">
    import { fly, slide } from "svelte/transition";
    import AvatarPlaceholder from "$lib/components/AvatarPlaceholder.svelte";
    import ActionButton from "$lib/components/ActionButton.svelte";
    import Icon from "$lib/components/Icon.svelte";

    interface Exercise {
        id: number;
        name: string;
        sets: number;
        reps: number;
        weight: number;
    }

    const SUGGESTIONS = [
        "Barbell Squat", "Bench Press", "Deadlift", "Overhead Press",
        "Barbell Row", "Pull-ups", "Lunges", "Lat Pulldown",
        "Leg Press", "Dumbbell Curl", "Tricep Extension", "Cable Fly",
        "Hip Thrust", "Romanian Deadlift", "Incline Bench Press",
    ];

    let { workout = $bindable({
        clientName: "",
        date: new Date().toISOString().split("T")[0],
        notes: "",
        exercises: [{ id: 1, name: "", sets: 3, reps: 10, weight: 0 }],
    }) } = $props();

    let editingName = $state(false);
    let showNotes = $state(false);
    let openExercises = $state<Set<number>>(new Set([1]));
    let activeAutocomplete = $state<number | null>(null);
    let autocompleteIndex = $state(-1);

    const inputs = {
        name: {} as Record<number, HTMLInputElement>,
        sets: {} as Record<number, HTMLInputElement>,
        reps: {} as Record<number, HTMLInputElement>,
        weight: {} as Record<number, HTMLInputElement>
    };
    let exerciseCards = {} as Record<number, HTMLElement>;

    function focusNext(type: keyof typeof inputs, id: number) {
        setTimeout(() => {
            inputs[type][id]?.focus();
            inputs[type][id]?.select();
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
                focusNext("sets", exercise.id);
                return;
            }
        }
        if (e.key === "Enter") {
            e.preventDefault();
            activeAutocomplete = null;
            focusNext("sets", exercise.id);
        }
    }

    function handleKeydown(e: KeyboardEvent, nextType: keyof typeof inputs, id: number) {
        if (e.key === "Enter") {
            e.preventDefault();
            focusNext(nextType, id);
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
            display: target.toLocaleDateString("en-US", { month: "short", day: "numeric", year: "numeric" })
        };
    });

    let autocompleteOptions = $derived((name: string) => {
        if (!name.trim()) return [];
        const lower = name.toLowerCase();
        return SUGGESTIONS.filter(s => s.toLowerCase().includes(lower) && s.toLowerCase() !== lower).slice(0, 5);
    });

    function toggleExercise(id: number) {
        const next = new Set(openExercises);
        next.has(id) ? next.delete(id) : next.add(id);
        openExercises = next;
    }

    function toggleAll(expand: boolean) {
        openExercises = expand ? new Set(workout.exercises.map(e => e.id)) : new Set();
    }

    function addExercise() {
        const id = Date.now();
        workout.exercises.push({ id, name: "", sets: 3, reps: 10, weight: 0 });
        openExercises = new Set([...openExercises, id]);

        setTimeout(() => {
            exerciseCards[id]?.scrollIntoView({ behavior: "smooth", block: "center" });
            inputs.name[id]?.focus();
        }, 80);
    }

    function removeExercise(id: number) {
        if (workout.exercises.length <= 1) return;
        workout.exercises = workout.exercises.filter(ex => ex.id !== id);
        openExercises.delete(id);
    }

    function step(exercise: Exercise, key: "sets" | "reps" | "weight", delta: number, min: number) {
        exercise[key] = Math.max(min, exercise[key] + delta);
    }
</script>

{#snippet Stepper(exercise: Exercise, field: "sets" | "reps" | "weight", label: string, min: number, stepSize: number, nextFocus: keyof typeof inputs | null)}
    <div class="flex flex-col items-center gap-1 w-full relative">
        <span class="text-[10px] tracking-widest text-base-content/50 uppercase">{label}</span>
        <div class="flex items-center justify-center gap-1 w-full">
            <button class="hidden sm:flex w-9 h-9 sm:w-10 sm:h-10 rounded-full items-center justify-center text-base-content/60 hover:bg-base-content/10 hover:text-primary active:scale-90 transition-all" onclick={() => step(exercise, field, -stepSize, min)}>−</button>
            <input 
                type="number" inputmode="numeric" min={min}
                class="w-full sm:w-14 h-11 sm:h-10 text-base sm:text-sm text-center bg-base-content/5 border border-base-content/10 rounded-lg text-base-content outline-none focus:border-primary/50 focus:bg-base-content/10 [appearance:textfield] [&::-webkit-inner-spin-button]:appearance-none transition-colors" 
                bind:value={exercise[field]} bind:this={inputs[field][exercise.id]} 
                onkeydown={(e) => nextFocus && handleKeydown(e, nextFocus, exercise.id)} 
                onfocus={(e) => e.currentTarget.select()} oncontextmenu={(e) => e.preventDefault()} 
            />
            <button class="hidden sm:flex w-9 h-9 sm:w-10 sm:h-10 rounded-full items-center justify-center text-base-content/60 hover:bg-base-content/10 hover:text-primary active:scale-90 transition-all" onclick={() => step(exercise, field, stepSize, min)}>+</button>
        </div>
        {#if label === 'Weight'}<span class="text-[10px] text-base-content/40 absolute -bottom-4">lbs</span>{/if}
    </div>
{/snippet}

<div class="w-full max-w-[580px] mx-auto px-4 py-6 sm:py-10 flex flex-col gap-5 font-sans">
    <header class="flex items-start justify-between gap-4">
        <div class="flex items-center gap-3 flex-1 min-w-0">
            <AvatarPlaceholder name={workout.clientName || "New Client"} rounded="full" />
            <div class="flex flex-col min-w-0">
                {#if editingName}
                    <input
                        class="bg-base-content/5 border border-primary/40 rounded-lg text-base-content px-3 py-1 text-sm outline-none w-44 font-medium"
                        placeholder="Client name…" autofocus bind:value={workout.clientName}
                        onblur={() => (editingName = false)} onkeydown={(e) => e.key === "Enter" && (editingName = false)}
                    />
                {:else}
                    <h1 class="text-2xl font-bold leading-tight m-0 truncate">{workout.clientName || "Client Name"}</h1>
                    <div class="flex items-center gap-1.5 text-xs mt-1">
                        <button class="text-primary font-medium hover:opacity-80 transition-opacity" onclick={() => (editingName = true)}>{dateInfo.label}</button>
                        <span class="text-base-content/50">·</span>
                        <label class="relative flex items-center gap-1 cursor-pointer group">
                            <span class="text-base-content/60 group-hover:text-base-content transition-colors">{dateInfo.display}</span>
                            <Icon name="edit" className="text-[12px] text-base-content/50 group-hover:text-base-content" />
                            <input type="date" class="absolute inset-0 opacity-0 cursor-pointer w-full" bind:value={workout.date} />
                        </label>
                    </div>
                {/if}
            </div>
        </div>
        <div class="flex gap-1 pt-1 shrink-0">
            <ActionButton icon="keyboard_double_arrow_up" color="btn-ghost" className="btn-sm btn-circle text-base-content/60 hover:text-primary hover:bg-base-content/10 border-none" onclick={() => toggleAll(false)} />
            <ActionButton icon="keyboard_double_arrow_down" color="btn-ghost" className="btn-sm btn-circle text-base-content/60 hover:text-primary hover:bg-base-content/10 border-none" onclick={() => toggleAll(true)} />
        </div>
    </header>

    <div class="h-px w-full bg-base-content/10"></div>

    <div class="flex flex-col gap-3">
        {#each workout.exercises as exercise, index (exercise.id)}
            {@const isOpen = openExercises.has(exercise.id)}
            <div bind:this={exerciseCards[exercise.id]} in:fly={{ y: -10, duration: 150 }} class="bg-base-content/5 border border-base-content/10 rounded-2xl overflow-visible transition-colors hover:border-base-content/20">
                <button class="w-full flex items-center gap-3 px-4 py-3.5 text-left select-none" onclick={() => toggleExercise(exercise.id)}>
                    <span class="shrink-0 w-6 h-6 rounded-md bg-base-content/10 text-base-content/60 text-xs font-semibold flex items-center justify-center">
                        {index + 1}
                    </span>
                    <span class="flex-1 text-sm font-medium text-base-content truncate">{exercise.name || "Untitled Exercise"}</span>
                    {#if !isOpen && exercise.name}
                        <span class="text-xs text-base-content/50 shrink-0 hidden sm:inline">
                            {exercise.sets}×{exercise.reps}{exercise.weight > 0 ? ` · ${exercise.weight} lbs` : ""}
                        </span>
                    {/if}
                    <Icon name="expand_more" className="text-lg text-base-content/50 shrink-0 transition-transform duration-200 {isOpen ? 'rotate-180' : ''}" />
                </button>

                {#if isOpen}
                    <div class="px-4 pb-4 pt-2 flex flex-col gap-5 border-t border-base-content/10" transition:slide={{ duration: 150 }}>
                        <div class="relative">
                            <input
                                type="text"
                                class="w-full bg-base-content/5 border border-base-content/10 rounded-xl text-base-content text-sm px-4 py-2.5 outline-none focus:border-primary/50 focus:bg-base-content/10 transition-all placeholder-base-content/40"
                                placeholder="Exercise name…"
                                bind:value={exercise.name} bind:this={inputs.name[exercise.id]}
                                onfocus={() => { activeAutocomplete = exercise.id; autocompleteIndex = -1; }}
                                onblur={() => setTimeout(() => { activeAutocomplete = null; autocompleteIndex = -1; }, 160)}
                                oninput={() => (autocompleteIndex = -1)}
                                onkeydown={(e) => handleNameKeydown(e, exercise)}
                            />
                            {#if activeAutocomplete === exercise.id && autocompleteOptions(exercise.name).length > 0}
                                <ul class="absolute left-0 right-0 top-[calc(100%+4px)] z-30 bg-base-200 border border-base-content/10 rounded-xl shadow-2xl py-1 overflow-hidden" in:fly={{ y: -4, duration: 120 }}>
                                    {#each autocompleteOptions(exercise.name) as s, i}
                                        <li>
                                            <button 
                                                class="w-full text-left px-4 py-2 text-sm flex items-center gap-2 transition-colors {autocompleteIndex === i ? 'bg-primary/20 text-base-content font-medium' : 'text-base-content/80 hover:bg-primary/10 hover:text-base-content'}" 
                                                onmousedown={() => { exercise.name = s; activeAutocomplete = null; autocompleteIndex = -1; focusNext('sets', exercise.id); }}
                                            >
                                                <Icon name="fitness_center" className="text-sm {autocompleteIndex === i ? 'text-primary' : 'text-primary/50'}" /> {s}
                                            </button>
                                        </li>
                                    {/each}
                                </ul>
                            {/if}
                        </div>

                        <div class="grid grid-cols-3 gap-2 items-start justify-items-center">
                            {@render Stepper(exercise, "sets", "Sets", 1, 1, "reps")}
                            {@render Stepper(exercise, "reps", "Reps", 0, 1, "weight")}
                            {@render Stepper(exercise, "weight", "Weight", 0, 5, null)}
                        </div>

                        {#if workout.exercises.length > 1}
                            <div class="flex justify-end pt-2">
                                <ActionButton icon="delete" color="btn-error" className="btn-sm btn-circle text-base-content/50 hover:text-error hover:bg-error/10 border-none opacity-50 hover:opacity-100 transition-opacity" onclick={() => removeExercise(exercise.id)} />
                            </div>
                        {/if}
                    </div>
                {/if}
            </div>
        {/each}
    </div>

    <button class="w-full py-3.5 rounded-2xl border border-dashed border-base-content/20 flex items-center justify-center gap-2 text-sm text-base-content/60 hover:text-primary hover:bg-base-content/5 hover:border-primary/40 transition-all" onclick={addExercise}>
        <Icon name="add" className="text-lg" /> Add Exercise
    </button>

    <div class="flex flex-col">
        <button class="flex items-center gap-2 text-sm text-base-content/60 hover:text-base-content w-max transition-colors" onclick={() => (showNotes = !showNotes)}>
            <Icon name="edit_note" className="text-lg" /> Session Notes
            <Icon name="expand_more" className="text-sm transition-transform duration-200 {showNotes ? 'rotate-180' : ''}" />
            {#if workout.notes && !showNotes}<span class="w-1.5 h-1.5 rounded-full bg-primary ml-1"></span>{/if}
        </button>
        {#if showNotes}
            <div transition:slide={{ duration: 200 }} class="pt-3">
                <textarea class="w-full bg-base-content/5 border border-base-content/10 rounded-xl text-base-content text-sm px-4 py-3 outline-none focus:border-primary/40 resize-y min-h-[80px] placeholder-base-content/40 transition-all" placeholder="How did it feel? Any pain or PRs?" bind:value={workout.notes}></textarea>
            </div>
        {/if}
    </div>

    <button class="btn btn-primary w-full h-14 mt-2 rounded-2xl text-base shadow-[0_4px_20px_var(--fallback-p,oklch(var(--p)/0.25))] active:scale-[0.98] transition-all" onclick={() => console.log("Saving:", workout)}>
        <Icon name="check" className="text-lg" /> Save Workout
    </button>
</div>
