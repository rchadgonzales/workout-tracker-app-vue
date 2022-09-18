<template>
  <div class="max-w-screen-sm mx-auto px-4 py-10">
    <!-- APP MESSAGE -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-7 p-4 rounded-md shadow-md bg-light-grey"
    >
      <p class="text-at-light-blue">
        {{ statusMsg }}
      </p>
      <p class="text-red-500">
        {{ errorMsg }}
      </p>
    </div>
    <div v-if="dataLoaded">
      <!-- GENERAL WORKOUT INFORMATION -->
      <div
        class="flex flex-col items-center p-8 rounded-md shadow-md bg-light-grey relative"
      >
        <div v-if="user" class="flex absolute left-2 top-2 gap-x-2">
          <div
            @click="editMode"
            class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer bg-at-light-blue shadow-lg"
          >
            <img
              src="@/assets/images/pencil-light.png"
              alt=""
              class="h-3.5 w-auto"
            />
          </div>
          <div
            @click="deleteWorkout"
            class="h-7 w-7 rounded-full flex justify-center items-center cursor-pointer bg-at-light-blue shadow-lg"
          >
            <img
              src="@/assets/images/trash-light.png"
              alt=""
              class="h-3.5 w-auto"
            />
          </div>
        </div>
        <img
          v-if="data.workoutType === 'cardio'"
          src="@/assets/images/running-light-green.png"
          alt=""
          class="h-24 w-auto"
        />
        <img
          v-else
          src="@/assets/images/dumbbell-light-green.png"
          alt=""
          class="h-24 w-auto"
        />
        <span
          class="mt-6 py-1.5 px-5 text-xs text-white bg-at-light-blue rounded-lg shadow-md"
        >
          {{ data.workoutType }}
        </span>
        <div class="w-full mt-6">
          <input
            v-if="edit"
            v-model="data.workoutName"
            type="text"
            class="p-2 w-full text-gray-500 focus:outline-none"
          />
          <h1 v-else class="text-at-light-blue text-2xl text-center">
            {{ data.workoutName }}
          </h1>
        </div>
      </div>

      <!-- EXERCISES -->
      <div
        class="mt-3 p-7 rounded-md flex flex-col items-center bg-light-grey shadow-md"
      >
        <!-- STRENGTH TRAINING -->
        <div
          v-if="data.workoutType === 'strength'"
          class="flex flex-col gap-y-4 w-full"
        >
          <div
            v-for="(item, index) in data.exercises"
            :key="index"
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label for="exercise-name" class="mb-1 text-sm text-at-light-blue"
                >Exercise</label
              >
              <input
                v-if="edit"
                id="exercise-name"
                v-model="item.exercise"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
              />
              <p v-else>{{ item.exercise }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="sets" class="mb-1 text-sm text-at-light-blue"
                >Sets</label
              >
              <input
                v-if="edit"
                id="sets"
                v-model="item.sets"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
              />
              <p v-else>{{ item.sets }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="reps" class="mb-1 text-sm text-at-light-blue"
                >Reps</label
              >
              <input
                v-if="edit"
                id="reps"
                v-model="item.reps"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
              />
              <p v-else>{{ item.reps }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="weight" class="mb-1 text-sm text-at-light-blue"
                >Weight (LB's)</label
              >
              <input
                v-if="edit"
                id="weight"
                v-model="item.weight"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
              />
              <p v-else>{{ item.weight }}</p>
            </div>
            <img
              @click="deleteExercise(item.id)"
              v-if="edit"
              src="@/assets/images/trash-light-green.png"
              alt=""
              class="absolute h-4 w-auto -left-5 cursor-pointer"
            />
          </div>
          <button
            @click="addExercise"
            v-if="edit"
            type="button"
            class="mt-6 py-2 px-6 rounded-md self-start text-sm text-white bg-at-light-blue duration-200 border-solid border-2 border-transparent hover:border-at-blue hover:bg-at-blue hover:text-white"
          >
            Add Exercise
          </button>
        </div>

        <!-- CARDIO -->
        <div v-else class="flex flex-col gap-y-4 w-full">
          <div
            v-for="(item, index) in data.exercises"
            :key="index"
            class="flex flex-col gap-x-6 gap-y-2 relative sm:flex-row"
          >
            <div class="flex flex-2 flex-col md:w-1/3">
              <label for="cardioType" class="mb-1 text-sm text-at-light-blue"
                >Type</label
              >
              <select
                v-if="edit"
                id="cardioType"
                v-model="item.cardioType"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
              >
                <option value="#">Select Type</option>
                <option value="run">Runs</option>
                <option value="walk">Walk</option>
              </select>
              <p v-else>{{ item.cardioType }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="distance" class="mb-1 text-sm text-at-light-blue"
                >Distance</label
              >
              <input
                v-if="edit"
                id="distance"
                v-model="item.distance"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
              />
              <p v-else>{{ item.distance }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="duration" class="mb-1 text-sm text-at-light-blue"
                >Duration</label
              >
              <input
                v-if="edit"
                id="duration"
                v-model="item.duration"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
              />
              <p v-else>{{ item.duration }}</p>
            </div>
            <div class="flex flex-1 flex-col">
              <label for="pace" class="mb-1 text-sm text-at-light-blue"
                >Pace</label
              >
              <input
                v-if="edit"
                id="pace"
                v-model="item.pace"
                type="text"
                class="p-2 w-full text-gray-500 focus:outline-none"
              />
              <p v-else>{{ item.pace }}</p>
            </div>
            <img
              @click="deleteExercise(item.id)"
              v-if="edit"
              src="@/assets/images/trash-light-green.png"
              alt=""
              class="absolute h-4 w-auto -left-5 cursor-pointer"
            />
          </div>
          <button
            @click="addExercise"
            v-if="edit"
            type="button"
            class="mt-6 py-2 px-6 rounded-md self-start text-sm text-white bg-at-light-blue duration-200 border-solid border-2 border-transparent hover:border-at-blue hover:bg-at-blue hover:text-white"
          >
            Add Exercise
          </button>
        </div>
      </div>

      <!-- UPDATE -->
      <button
        @click="update"
        v-if="edit"
        type="button"
        class="mt-6 py-2 px-6 rounded-md self-start text-sm text-white bg-at-light-blue duration-200 border-solid border-2 border-transparent hover:border-at-blue hover:bg-at-blue hover:text-white"
      >
        Update Workout
      </button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { supabase } from "../supabase/init";
import { useRoute, useRouter } from "vue-router";
import store from "../store/index";
import { uid } from "uid";
export default {
  name: "view-workout",
  setup() {
    // CREATE DATA / VARS
    const data = ref(null);
    const dataLoaded = ref(null);
    const errorMsg = ref(null);
    const statusMsg = ref(null);
    const route = useRoute();
    const router = useRouter();
    const user = computed(() => store.state.user);

    // GET CURRENT ID OF ROUTE
    const currentId = route.params.workoutId;

    // GET WORKOUT DATA
    const getData = async () => {
      try {
        const { data: workouts, error } = await supabase
          .from("workouts")
          .select("*")
          .eq("id", currentId);
        if (error) throw error;
        data.value = workouts[0];
        dataLoaded.value = true;
        console.log(data.value);
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 7000);
      }
    };

    // RUN DATA FUNCTION
    getData();

    // DELETE WORKOUT
    const deleteWorkout = async () => {
      try {
        const { error } = await supabase
          .from("workouts")
          .delete()
          .eq("id", currentId);
        if (error) throw error;
        router.push({ name: "Home" });
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 7000);
      }
    };

    // EDIT MODE
    const edit = ref(null);
    const editMode = () => {
      edit.value = !edit.value;
    };

    // ADD EXERCISE
    const addExercise = () => {
      if (data.value.workoutType === "strength") {
        data.value.exercises.push({
          id: uid(),
          exercise: "",
          sets: "",
          reps: "",
          weight: "",
        });
        return;
      }
      data.value.exercises.push({
        id: uid(),
        cardioType: "",
        distance: "",
        duration: "",
        pace: "",
      });
    };

    // DELETE EXERCISE
    const deleteExercise = (id) => {
      if (data.value.exercises.length > 1) {
        data.value.exercises = data.value.exercises.filter(
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

    // UPDATE WORKOUT
    const update = async () => {
      try {
        const { error } = await supabase
          .from("workouts")
          .update({
            workoutName: data.value.workoutName,
            exercises: data.value.exercises,
          })
          .eq("id", currentId);
        if (error) throw error;
        edit.value = false;
        statusMsg.value = "Success: Workout has been updated";
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
      statusMsg,
      data,
      dataLoaded,
      errorMsg,
      edit,
      editMode,
      user,
      deleteWorkout,
      addExercise,
      deleteExercise,
      update,
    };
  },
};
</script>
