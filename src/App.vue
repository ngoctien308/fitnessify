<script setup>
    import Welcome from '@/components/pages/Welcome.vue';
    import Dashboard from '@/components/pages/Dashboard.vue';
    import Workout from '@/components/pages/Workout.vue';
    import Layout from '@/components/layouts/Layout.vue';
    import { ref } from 'vue';
    import { workoutProgram } from './utils';

    const selectedPage = ref(1); // 1: Welcome, 2: Dashboard, 3: Workout
    const selectedWorkout = ref(-1); // -1 indicates no workout selected
    const workoutData = ref({}); // To store workout progress

    const handleChangePage = (pageNumber) => {
        selectedPage.value = pageNumber;
    };

    const handleChangeWorkout = (workoutIndex) => {
        selectedPage.value = 3; // Switch to Workout page
        selectedWorkout.value = workoutIndex;
    };

    const handleSaveWorkout = () => {
        localStorage.setItem('workoutData', JSON.stringify(workoutData.value));
        // Logic to save workout progress
        selectedPage.value = 2; // Return to Dashboard after saving
        selectedWorkout.value = -1; // Reset selected workout
    };
</script>

<template>    
    <Layout>
        <Welcome :handleChangePage="handleChangePage" v-if="selectedPage == 1" />
        <Dashboard :handleChangeWorkout="handleChangeWorkout" v-if="selectedPage == 2" />
        <Workout
            :handleSaveWorkout="handleSaveWorkout"          
            :selectedWorkout="selectedWorkout" 
            v-if="selectedPage == 3 && workoutProgram?.[selectedWorkout]" 
        />
    </Layout>
</template>
