<template>
  <div class="max-w-screen-md mx-auto px-4 py-10">
    <!-- STATUS MESSAGE -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 bg-light-grey rounded-md shadow-lg"
    >
      <p class="text-at-light-blue">{{ statusMsg }}</p>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>
    <!-- CREATE FORM -->
    <div class="p-8 flex items-start bg-light-grey rounded-md shadow-lg">
      <!-- FORM -->
      <form
        @submit.prevent="createWorkout"
        class="flex flex-col gap-y-5 w-full"
      >
        <h1 class="text-2xl text-at-blue">Record Workout</h1>
        <!-- WORKOUT NAME -->
        <div class="flex flex-col">
          <label for="workout-name" class="mb-1 text-sm text-at-light-blue"
            >Workout Name</label
          >
          <input
            type="text"
            required
            class="p-2 text-gray-500 focus:outline-none"
            id="workout-name"
            v-model="workoutName"
          />
        </div>

        <!-- WORKOUT TYPE -->
        <div class="flex flex-col">
          <label for="workout-type" class="mb-1 text-sm text-at-light-blue"
            >Workout Type</label
          >
          <select
            @change="workoutChange"
            id="workout-type"
            class="p-2 text-gray-500 focus:outline-none"
            required
            v-model="workoutType"
          >
            <option value="select-workout">Select Workout</option>
            <option value="strength">Strength Training</option>
            <option value="cardio">Cardio</option>
          </select>
        </div>

        <!-- STRENGTH TRAINING INPUTS -->
        <div v-if="workoutType === 'strength'" class="flex flex-col gap-y-4">
          <div
            v-for="(item, index) in exercises"
            :key="index"
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
          >
            <div class="flex flex-col md:w-1/3">
              <label for="exercise-name" class="mb-1 text-sm text-at-light-blue"
                >Exercise</label
              >
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.exercise"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="sets" class="mb-1 text-sm text-at-light-blue"
                >Sets</label
              >
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.sets"
              />
            </div>
            <div class="flex flex-col md:w-1/3">
              <label for="reps" class="mb-1 text-sm text-at-light-blue"
                >Reps</label
              >
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.reps"
              />
            </div>
            <div class="flex flex-col md:w-1/3">
              <label for="weight" class="mb-1 text-sm text-at-light-blue"
                >Weight (LB's)</label
              >
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.weight"
              />
            </div>
            <img
              @click="deleteExercise(item.id)"
              src="@/assets/images/trash-light-green.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt=""
            />
          </div>
          <button
            @click="addExercise"
            type="button"
            class="mt-6 py-2 px-6 rounded-md self-start text-sm text-white bg-at-light-blue duration-200 border-solid border-2 border-transparent hover:border-at-blue hover:bg-at-blue hover:text-white"
          >
            Add Exercise
          </button>
        </div>
        <!-- CARDIO INPUTS -->
        <div v-if="workoutType === 'cardio'" class="flex flex-col gap-y-4">
          <div
            v-for="(item, index) in exercises"
            :key="index"
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
          >
            <div class="flex flex-col md:w-1/3">
              <label for="cardio-type" class="mb-1 text-sm text-at-light-blue"
                >Type</label
              >
              <select
                id="cardio-type"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.cardioType"
              >
                <option value="#">Select Type</option>
                <option value="run">Runs</option>
                <option value="walk">Walk</option>
              </select>
            </div>
            <div class="flex flex-col flex-1">
              <label for="distance" class="mb-1 text-sm text-at-light-blue"
                >Distance</label
              >
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.distance"
              />
            </div>
            <div class="flex flex-col md:w-1/3">
              <label for="duration" class="mb-1 text-sm text-at-light-blue"
                >Duration</label
              >
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.duration"
              />
            </div>
            <div class="flex flex-col md:w-1/3">
              <label for="pace" class="mb-1 text-sm text-at-light-blue"
                >Pace</label
              >
              <input
                type="text"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.pace"
              />
            </div>
            <img
              @click="deleteExercise(item.id)"
              src="@/assets/images/trash-light-green.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
              alt=""
            />
          </div>
          <button
            @click="addExercise"
            type="button"
            class="mt-6 py-2 px-6 rounded-md self-start text-sm text-white bg-at-light-blue duration-200 border-solid border-2 border-transparent hover:border-at-blue hover:bg-at-blue hover:text-white"
          >
            Add Exercise
          </button>
        </div>
        <button
          type="submit"
          class="mt-6 py-2 px-6 rounded-md self-start text-sm text-white bg-at-light-blue duration-200 border-solid border-2 border-transparent hover:border-at-blue hover:bg-at-blue hover:text-white"
        >
          Record Workout
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { uid } from "uid";
import { supabase } from "../supabase/init";
export default {
  name: "create",
  setup() {
    // CREATE DATA
    const workoutName = ref("");
    const workoutType = ref("select-workout");
    const exercises = ref([]);
    const statusMsg = ref(null);
    const errorMsg = ref(null);

    // ADD EXERCISE
    const addExercise = () => {
      if (workoutType.value === "strength") {
        exercises.value.push({
          id: uid(),
          exercise: "",
          sets: "",
          reps: "",
          weight: "",
        });
        return;
      }
      exercises.value.push({
        id: uid(),
        cardioType: "",
        distance: "",
        duration: "",
        pace: "",
      });
    };

    // DELETE EXERCISE
    const deleteExercise = (id) => {
      if (exercises.value.length > 1) {
        exercises.value = exercises.value.filter(
          (exercise) => exercise.id !== id
        );
        return;
      }
      errorMsg.value =
        "Error: Cannot remove, need to have at least one exercise";
      setTimeout(() => {
        errorMsg.value = false;
      }, 7000);
    };

    // LISTENS FOR CHANGING OF WORKOUT TYPE INPUT
    const workoutChange = () => {
      exercises.value = [];
      addExercise();
    };

    // CREATE WORKOUT
    const createWorkout = async () => {
      try {
        const { error } = await supabase.from("workouts").insert([
          {
            workoutName: workoutName.value,
            workoutType: workoutType.value,
            exercises: exercises.value,
          },
        ]);
        if (error) throw error;
        statusMsg.value = "Success: Workout has been created";
        workoutName.value = null;
        workoutType.value = "select-workout";
        exercises.value = [];
        setTimeout(() => {
          statusMsg.value = false;
        }, 7000);
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 7000);
      }
    };

    return {
      workoutName,
      workoutType,
      exercises,
      statusMsg,
      errorMsg,
      addExercise,
      workoutChange,
      deleteExercise,
      createWorkout,
    };
  },
};
</script>
