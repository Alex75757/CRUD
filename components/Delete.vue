<template>
  <div  class="edit-form py-3">
    <p class="headline">Are you shure? <br>
        Delete All Tutorials</p>

    <v-form ref="form" lazy-validation>
     
       

      <v-btn color="error" small class="mr-2" @click="deleteAllTutorials">
        Delete
      </v-btn>

      

       <v-btn
            color="orange lighten-3"
            small
            nuxt
            to="/read"
          >
            Cancel
          </v-btn>

    </v-form>

    <p class="mt-3">{{ message }}</p>
  </div>
 
</template>

<script>
import TutorialDataService from "../app/services/TutorialDataService";

export default {
  name: "tutorial",
  data() {
    return {
      currentTutorial: null,
      message: "",
    };
  },
  methods: {
  
  deleteAllTutorials() {
    TutorialDataService.deleteAll()
        .then((response) => {
          console.log(response.data);
          this.$router.push({ name: "index" });
        })
        .catch((e) => {
          console.log(e);
        });


      
         
    },
  },
  
  
};
</script>

<style>
.edit-form {
  max-width: 380px;
  margin: auto;
  box-shadow: 1rem;
}
</style>