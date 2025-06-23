<script setup lang="ts">
import Welcome from './components/pages/Welcome.vue';
import Layout from './components/layouts/Layout.vue';
import Dashboard from './components/pages/Dashboard.vue';
import Workout from './components/pages/Workout.vue';
import { ref, computed, onMounted } from 'vue';
import { workoutProgram } from './utils/index'

const selectedDisplay = ref(1);

const defaultData:Record<string, any> = {};


for (let workoutIdx in workoutProgram) {
  const workoutData = workoutProgram[workoutIdx];
  defaultData[workoutIdx] = {};

  for (let e of workoutData.workout) {
    defaultData[workoutIdx][e.name] = '';
  }

}

const data = ref(defaultData);

const selectedWorkout = ref(-1);

const isWorkoutComplete = computed(() => {
  const currWorkout = data.value?.[selectedWorkout.value];
  if (!currWorkout) return false;

  const isCompletedCheck = Object.values(currWorkout).every(ex => !!ex);
  return isCompletedCheck;
});

const firstIncompletedWorkoutIndex = computed(() => {
  const allWorkouts = data.value;
  if (!allWorkouts) return -1;

  for (const [index, workout] of Object.entries(allWorkouts)) {
    const isCompleted = Object.values(workout).every(ex => !!ex);
    if (!isCompleted) {
      return parseInt(index, 10);
    }
  }

  return -1;
});

const handleDisplayChange = (display: number) => {
  selectedDisplay.value = display;
};

const handleWorkoutChange = (workout: number) => {
  selectedDisplay.value = 3;
  selectedWorkout.value = workout;
};

const handleSaveWorkout = () => {
  localStorage.setItem('workouts', JSON.stringify(data.value));
  selectedDisplay.value = 2;
  selectedWorkout.value = -1;
};

const handledResetPlan = () => {
  selectedDisplay.value = 2;
  selectedWorkout.value = -1;
  data.value = defaultData;
  localStorage.removeItem('workouts');
}

onMounted(() => {
  if (!localStorage) return;
  if (localStorage.getItem('workouts')) {
    const savedData = JSON.parse(localStorage.getItem('workouts') ?? '');
    data.value = savedData
    selectedDisplay.value = 2;
  }
});

</script>

<template>
  <Layout>

    <Welcome :handleDisplayChange="handleDisplayChange" v-if="selectedDisplay === 1" />

    <Dashboard :handledResetPlan="handledResetPlan" :firstIncompletedWorkoutIndex="firstIncompletedWorkoutIndex"
      :handleDisplayChange="handleDisplayChange" :handleWorkoutChange="handleWorkoutChange" :data="data"
      v-else-if="selectedDisplay === 2" />

    <Workout :selectedWorkout="selectedWorkout" :handleDisplayChange="handleDisplayChange"
      :handleWorkoutChange="handleWorkoutChange" :handleSaveWorkout="handleSaveWorkout" :data="data"
      :isWorkoutComplete="isWorkoutComplete" v-else-if="selectedDisplay === 3" />
  </Layout>
</template>

<style scoped></style>
