<script setup>
import { computed, ref } from 'vue';
import { workoutProgram, exerciseDescriptions } from '../../utils';
import Portal from '../Portal.vue';

const workoutTypes = ['Push', 'Pull', 'Legs'];


const {selectedWorkout} = defineProps({
    handleDisplayChange: {
        type: Function,
        required: true
    },
    handleWorkoutChange: {
        type: Function,
        required: true
    },
    data: {
        type: Object,
        required: true
    },
    handleSaveWorkout: {
        type: Function,
        required: true
    },
    isWorkoutComplete: {
        type: Boolean,
        required: true
    },
    selectedWorkout: {
        type: Number,
        required: true
    }
});

console.log("selectedWorkout", selectedWorkout);

const { workout, warmup } = workoutProgram[selectedWorkout];
let selectedExercise = ref(null);
const defaultExerciseDescription = 'No description available for this exercise.';
let exerciseDescription = computed(() => {
    return exerciseDescriptions[selectedExercise.value] || defaultExerciseDescription;
});

const handleCloseModal = () => {
    selectedExercise.value = null;
}

</script>

<template>
    <Portal :handleCloseModal="handleCloseModal" v-if="selectedExercise">
        <div class="exercise-description">
            <h4>{{ selectedExercise }}</h4>
            <div>
                <small>Description</small>
                <p>{{ exerciseDescription }}</p>
            </div>
            <button @click="handleCloseModal">Close <i class="fa-solid fa-xmark"></i></button>
        </div>
    </Portal>
    <section id="workout-card">
        <div class="plan-card card">
            <div class="plan-card-header">
                <h3>Day {{ selectedWorkout < 9 ? '0' + (selectedWorkout + 1) : selectedWorkout + 1 }}</h3>
                        <i class="fa-solid fa-dumbbell" v-if="selectedWorkout % 3 === 0"></i>
                        <i class="fa-solid fa-weight-hanging" v-else-if="selectedWorkout % 3 === 1"></i>
                        <i class="fa-solid fa-bolt" v-else-if="selectedWorkout % 3 === 2"></i>
            </div>
            <h2>{{ workoutTypes[selectedWorkout % 3]}} Workout</h2>
        </div>
        <div class="workout-grid">
            <h4 class="grid-name">Warmup</h4>
            <h6>Sets</h6>
            <h6>Reps</h6>
            <h6 class="grid-weights">Weights</h6>
            <div :key="wIdx" class="workout-grid-row" v-for="(w, wIdx) in warmup">
                <div class="grid-name">
                    <p>{{ w.name }}</p>
                    <button @click="() => {
                        selectedExercise = w.name;
                    }">
                        <i class="fa-regular fa-circle-question"></i>
                    </button>
                </div>
                <p>{{ w.sets }}</p>
                <p>{{ w.reps }}</p>
                <input class="grid-weights" type="text" placeholder="14kg" disabled />
            </div>
            <div class="workout-grid-line">

            </div>
            <h4 class="grid-name">Workout</h4>
            <h6>Sets</h6>
            <h6>Reps</h6>
            <h6 class="grid-weights">Weights</h6>
            <div :key="wIdx" class="workout-grid-row" v-for="(w, wIdx) in workout">
                <div class="grid-name">
                    <p>{{ w.name }}</p>
                    <button @click="() => {
                        selectedExercise = w.name;
                    }">
                        <i class="fa-regular fa-circle-question"></i>
                    </button>
                </div>
                <p>{{ w.sets }}</p>
                <p>{{ w.reps }}</p>
                <input v-model="data[selectedWorkout][w.name]" class="grid-weights" type="text" placeholder="14kg" />
            </div>
        </div>
        <div class="card workout-btns">
            <button @click="handleSaveWorkout">Save & Exit <i class="fa-solid fa-save"></i></button>
            <button @click="handleSaveWorkout" :disabled="!isWorkoutComplete" >Complete <i class="fa-solid fa-check"></i></button>
        </div>
    </section>
</template>

<style scoped>
#workout-card,
.plan-card {
    display: flex;
    flex-direction: column;
}

#workout-card {
    gap: 1.5rem;
}

.plan-card-header,
.workout-btns {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
}

.workout-grid,
.workout-grid-row {
    display: grid;
    grid-template-columns: repeat(7, minmax(0, 1fr));
    gap: 1rem;
}

.workout-grid-row,
.workout-grid-line {
    grid-column: span 7 / span 7;
}

.workout-grid-line {
    margin: 0.5rem 0;
    height: 3px;
    border-radius: 2px;
    background: var(--background-muted);
}

.grid-name {
    grid-column: span 3 / span 3;
    display: flex;
    align-items: center;
    gap: 1rem;
}

.grid-name button {
    padding: 0;
    border: none;
    box-shadow: none;
}

.grid-name button:hover {
    transform: none;
    box-shadow: none;
    color: var(--color-link);
}

.workout-grid-row .grid-name button {
    opacity: 0;
    pointer-events: none;
}

.workout-grid-row:hover .grid-name button {
    opacity: 1;
    pointer-events: all;
}

.grid-name p {
    text-transform: capitalize;
}

.grid-weights {
    grid-column: span 2 / span 2;
}

.workout-btns button {
    flex: 1;
}

.exercise-description {
    display: flex;
    flex-direction: column;
    gap: 1rem;
}

.exercise-description h3 {
    text-transform: capitalize;
}

.exercise-description button {
    padding-left: 0.5rem;
}
</style>