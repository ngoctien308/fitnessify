<script setup>
    import Welcome from '@/components/pages/Welcome.vue';
    import Dashboard from '@/components/pages/Dashboard.vue';
    import Workout from '@/components/pages/Workout.vue';
    import Layout from '@/components/layouts/Layout.vue';

    const chosenPage = 3; // 1: Welcome, 2: Dashboard, 3: Workout
</script>

<template>    
    <Layout>
        <Welcome v-if="chosenPage == 1" />
        <Dashboard v-if="chosenPage == 2" />
        <Workout v-if="chosenPage == 3" />
    </Layout>
</template>
