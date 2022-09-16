<template>
  <div v-if="appReady" class="min-h-full font-BarlowSC box-border">
    <Navigation />
    <router-view />
  </div>
</template>

<script>
import Navigation from "./components/Navigation.vue";
import { ref } from "vue";
import { supabase } from "./supabase/init";
import store from "./store/index";
export default {
  components: {
    Navigation,
  },
  setup() {
    // CREATE DATA / VARS
    const appReady = ref(null);

    // CHECK TO SEE IF USER IS ALREADY LOGGED IN
    const user = supabase.auth.user();

    // IF USER DOES NOT EXIST, NEED TO MAKE APP READy
    if (!user) {
      appReady.value = true;
    }

    // Runs when there is a auth state change
    // if user is logged in, this will fire
    supabase.auth.onAuthStateChange((_, session) => {
      console.log("hello");
      store.methods.setUser(session);
      appReady.value = true;
    });
    return { appReady };
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Barlow+Semi+Condensed:wght@100;200;300;400;500;600;700;800&display=swap");
</style>
