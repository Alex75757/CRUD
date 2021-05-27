<template>
  <div v-if="currentTutorial" class="edit-form py-4">
    <p class="headline">Edit Tutorial</p>

    <v-form ref="form" lazy-validation>
      <v-text-field
        v-model="currentTutorial.title"
        :rules="[(v) => !!v || 'Title is required']"
        label="Title"
        required
      ></v-text-field>

      <v-text-field
        v-model="currentTutorial.description"
        :rules="[(v) => !!v || 'Description is required']"
        label="Description"
        required
      ></v-text-field>

      <label><strong>Status:</strong></label>
      {{ currentTutorial.published ? "Published" : "Pending" }}

      <v-divider class="my-5"></v-divider>
      <v-row class="own"
            
            >
            <v-btn v-if="currentTutorial.published"
                @click="updatePublished(false)"
                color="primary" small class="mr-2"
            >
                UnPublish
            </v-btn>

            <v-btn v-else
                @click="updatePublished(true)"
                color="primary" small class="mr-2"
            >
                Publish
            </v-btn>

            <v-btn color="error" small class="mr-2" @click="deleteTutorial">
                Delete
            </v-btn>

            <v-btn color="success" small @click="updateTutorial">
                Update
            </v-btn>

            <v-btn
                    color="orange lighten-3"
                    small
                    nuxt
                    to="/read"
                >
                    Continue
                </v-btn>
        </v-row>
    </v-form>

    <p class="mt-3">{{ message }}</p>
  </div>

  <div v-else>
      <img src="../assets/clown.svg" alt="ups" width='300px'>
    <p>Selected nothing</p>
    <p>Please click on a Continue...</p>
       <v-btn
            color="orange lighten-3"
            large
            nuxt
            to="/read"
          >
            Continue
          </v-btn>
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
    getTutorial(id) {
      TutorialDataService.get(id)
        .then((response) => {
          this.currentTutorial = response.data;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },

    updatePublished(status) {
      var data = {
        id: this.currentTutorial.id,
        title: this.currentTutorial.title,
        description: this.currentTutorial.description,
        published: status,
      };

      TutorialDataService.update(this.currentTutorial.id, data)
        .then((response) => {
          this.currentTutorial.published = status;
          console.log(response.data);
        })
        .catch((e) => {
          console.log(e);
        });
    },

    updateTutorial() {
      TutorialDataService.update(this.currentTutorial.id, this.currentTutorial)
        .then((response) => {
          console.log(response.data);
          this.message = "The tutorial was updated successfully!";
        })
        .catch((e) => {
          console.log(e);
        });
    },

    deleteTutorial() {
      TutorialDataService.delete(this.currentTutorial.id)
        .then((response) => {
          console.log(response.data);
          this.$router.push({ name: "read" });
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
  
  mounted() {
    this.message = "";
    this.getTutorial(this.$route.params.id);
  },
};
</script>

<style>
.edit-form {
  max-width: 400px;
  margin: auto;
}
.own {
    margin: 0 auto;
    display: flex;
    /* align-content: center; */
    justify-content: space-between;
}
</style>