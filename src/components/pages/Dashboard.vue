<script setup>
    import { ref } from 'vue';
    import { gymHealthFacts } from '@/utils';
    import Grid from '@/components/Grid.vue';

    const todaysFact = ref(gymHealthFacts[Math.floor(Math.random() * gymHealthFacts.length)]);

    const { handleChangeWorkout, firstInCompleteWorkoutIndex, handleResetWorkouts } = defineProps({
        handleChangeWorkout: Function,
        firstInCompleteWorkoutIndex: Number,
        handleResetWorkouts: Function
    });
</script>

<template>
    <section id="dashboard">
        <div class="card tip-container">
            <h2>Welcome Smoldier</h2>
            <div>
                <p class="tip"><strong>Daily Tip</strong><br/>{{ todaysFact }}</p>
            </div>
            <button @click="() => handleChangeWorkout(firstInCompleteWorkoutIndex)">Start workout &rarr;</button>
        </div>
        <Grid :handleResetWorkouts="handleResetWorkouts" :firstInCompleteWorkoutIndex="firstInCompleteWorkoutIndex" :handleChangeWorkout="handleChangeWorkout" />
    </section>
</template>

<style scoped>
    .tip-container,
    .tip-container div,
    #dashboard {
        display: flex;
    }

    .tip-container,
    #dashboard {
        flex-direction: column;
    }

    #dashboard {
        gap: 2rem;
    }

    .tip-container {
        gap: 0.5rem;
    }

    @media (min-width: 640px) {
        .tip-container {
            gap: 1rem;
        }
    }
</style>
