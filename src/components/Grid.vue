<script setup>
    import { workoutProgram } from '@/utils';
    import { workoutTypes } from '@/utils';

    const { handleChangeWorkout, firstInCompleteWorkoutIndex, handleResetWorkouts } = defineProps({
        handleChangeWorkout: Function,
        firstInCompleteWorkoutIndex: Number,
        handleResetWorkouts: Function
    });
</script>

<template>
    <section id="grid">
        <button 
            :disabled="workoutIndex > firstInCompleteWorkoutIndex"
            @click="() => {
                handleChangeWorkout(workoutIndex);
            }"
            :key="workoutIndex"
            v-for="(workoutItem, workoutIndex) in Object.keys(workoutProgram)" class="card-button plan-card">
            <div>
                <p>Day {{ workoutIndex + 1 <= 9 ? `0${workoutIndex + 1}` : workoutIndex + 1 }}</p>
                <i class="fa-solid fa-dumbbell" v-if="workoutIndex % 3 == 0" ></i>
                <i class='fa-solid fa-weight-hanging' v-if="workoutIndex % 3 == 1"></i>
                <i class="fa-solid fa-bolt" v-if="workoutIndex % 3 == 2"></i>
            </div>
            <h3>{{ workoutTypes[workoutIndex % workoutTypes.length] }}</h3>
        </button>
        <button @click="handleResetWorkouts" class="card-button plan-card-reset">
           <p>Reset</p>
           <i class="fa-solid fa-rotate-left"></i>
        </button>
    </section>
</template>

<style scoped>
    #grid {
        display: grid;
        grid-template-columns: repeat(3, minmax(0, 1fr));
        gap: 1rem;
    }

    #grid button {
        width: 100%;
    }

    #grid button:disabled {
        box-shadow: none;
        cursor: not-allowed;

    }

    .plan-card{
        display: flex;
        flex-direction: column;
    }

    .plan-card-reset {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 1rem;
    }

    .plan-card div {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 0.5rem;
    }

    .plan-card div p {
        text-align: left;
    }

    @media (min-width: 640px) {
        #grid {
            grid-template-columns: repeat(4, minmax(0, 1fr));
        }
    }
</style>
