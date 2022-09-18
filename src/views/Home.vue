<template>
  <div v-if="dataLoaded" class="container mt-10 px-4">
    <!-- NO DATA -->
    <div v-if="data.length === 0" class="w-full flex flex-col items-center">
      <h1 class="text-2xl">Looks empty here. Create workout.</h1>
      <router-link
        :to="{ name: 'Create' }"
        class="mt-6 py-2 px-6 rounded-md text-sm text-white bg-at-light-blue duration-200 border-solid border-2 border-transparent hover:border-at-blue hover:bg-at-blue hover:text-white"
        >Create Workout</router-link
      >
    </div>
    <!-- DATA -->
    <div
      v-else
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
    >
      <router-link
        :to="{ name: 'View-Workout', params: { workoutId: workout.id } }"
        v-for="(workout, index) in data"
        :key="index"
        class="flex flex-col items-center bg-light-grey p-8 shadow-md cursor-pointer"
      >
        <!-- CARDIO IMAGE -->
        <img
          v-if="workout.workoutType === 'cardio'"
          src="@/assets/images/running-light-green.png"
          alt=""
          class="h-24 w-auto"
        />
        <!-- STRENGTH TRAINING IMAGE -->
        <img
          v-else
          src="@/assets/images/dumbbell-light-green.png"
          alt=""
          class="h-24 w-auto"
        />

        <p
          class="mt-6 py-1 px-3 text-xs text-white bg-at-light-blue shadow-md rounded-lg"
        >
          {{ workout.workoutType }}
        </p>
        <h1 class="mt-8 mb-2 text-center text-xl text-at-light-blue">
          {{ workout.workoutName }}
        </h1>
      </router-link>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { supabase } from "../supabase/init";
export default {
  name: "Home",
  components: {},
  setup() {
    // CREATE DATA / VARS
    const data = ref([]);
    const dataLoaded = ref(null);
    const errorMsg = ref(null);

    // GET DATA
    const getData = async () => {
      try {
        const { data: workouts, error } = await supabase
          .from("workouts")
          .select("*");
        if (error) throw error;
        data.value = workouts;
        dataLoaded.value = true;
        // console.log(data.value);
      } catch (error) {
        // console.warn(error.message);
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 7000);
      }
    };

    // RUN DATA FUNCTION
    getData();

    return { data, dataLoaded, errorMsg };
  },
};
</script>
