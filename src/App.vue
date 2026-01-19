<script setup>
    import Welcome from '@/components/pages/Welcome.vue';
    import Dashboard from '@/components/pages/Dashboard.vue';
    import Workout from '@/components/pages/Workout.vue';
    import Layout from '@/components/layouts/Layout.vue';
    import { computed, ref } from 'vue';
    import { workoutProgram } from './utils';

    const selectedPage = ref(1); // 1: Welcome, 2: Dashboard, 3: Workout
    const selectedWorkout = ref(-1); // -1 indicates no workout selected
    const defaultWorkoutData = {}; // Default structure for workout data
    
    const createDefaultWorkoutData = () => {
        const defaultWorkoutData = {};
        for(let workoutIndex in workoutProgram) {
            defaultWorkoutData[workoutIndex] = {};
            
            for(let excercise of workoutProgram[workoutIndex].workout) {
                defaultWorkoutData[workoutIndex][excercise.name] = '';
            }
        }
        return defaultWorkoutData;
    };

    const workoutData = ref(createDefaultWorkoutData()); // To store workout progress

    const isWorkoutComplete = computed(() => {        
        const currentWorkout = workoutData.value[selectedWorkout.value];
        if(!currentWorkout) return false;
        const isAllComplete = Object.values(currentWorkout).every(value => value !== '');
        return isAllComplete;
    });
    
    const firstInCompleteWorkoutIndex = computed(() => {
        for(const [index, workout] of Object.entries(workoutData.value)) {    
              const isAllComplete = Object.values(workout).every(value => value !== '');
                if(!isAllComplete) {
                    return Number(index);
                }
        }
        return 0;
    });

    const handleChangePage = (pageNumber) => {
        selectedPage.value = pageNumber;
    };

    const handleChangeWorkout = (workoutIndex) => {
        selectedPage.value = 3; // Switch to Workout page
        selectedWorkout.value = workoutIndex;
    };

    const handleSaveWorkout = () => {
        selectedPage.value = 2; // Return to Dashboard after saving
        selectedWorkout.value = -1; // Reset selected workout
    };

    const handleResetWorkouts = () => {
        workoutData.value = createDefaultWorkoutData();
        selectedPage.value = 2; // Return to Dashboard after resetting
        selectedWorkout.value = -1; // Reset selected workout
    };    
</script>

<template>    
    <Layout :handleChangePage="handleChangePage">
        <Welcome :handleChangePage="handleChangePage" v-if="selectedPage == 1" />
        <Dashboard
            :handleResetWorkouts="handleResetWorkouts"
            :firstInCompleteWorkoutIndex="firstInCompleteWorkoutIndex"
            :handleChangeWorkout="handleChangeWorkout" 
            v-if="selectedPage == 2" 
        />
        <Workout
            :isWorkoutComplete="isWorkoutComplete"
            :workoutData="workoutData"
            :handleSaveWorkout="handleSaveWorkout"     
            :selectedWorkout="selectedWorkout" 
            v-if="selectedPage == 3 && workoutProgram?.[selectedWorkout]"
        />
    </Layout>
</template>
