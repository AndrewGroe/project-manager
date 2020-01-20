<template>
  <div class="dashboard">
    <h1 subheading>Dashboard</h1>

    <v-container class="my-5">
      <v-layout row class="ma-3">
        <span class="mr-4">Sort By: </span>
        <v-tooltip top>
          <template v-slot:activator="{ on }">
            <v-btn v-on="on" small @click="sortBy('title')">
              <v-icon small left>mdi-folder</v-icon>
              <span class="caption text-lowercase">name</span>
            </v-btn>
          </template>
          <span>Sort projects by project name.</span>
        </v-tooltip>

        <v-tooltip top>
          <template v-slot:activator="{ on }">
            <v-btn v-on="on" small @click="sortBy('person')">
              <v-icon small left>mdi-account</v-icon>
              <span class="caption text-lowercase">person</span>
            </v-btn>
          </template>
          <span>Sort projects by person name.</span>
        </v-tooltip>
      </v-layout>

      <v-card class="pl-3" v-for="project in projects" :key="project.title">
        <v-layout row wrap :class="`pa-3 project ${project.status}`">
          <v-flex xs12 md6>
            <div class="caption">Project Title</div>
            <div>{{ project.title }}</div>
          </v-flex>

          <v-flex xs6 sm4 md2>
            <div class="caption">Person</div>
            <div>{{ project.person }}</div>
          </v-flex>

          <v-flex xs6 sm4 md2>
            <div class="caption">Due</div>
            <div>{{ project.due }}</div>
          </v-flex>

          <v-flex xs2 sm4 md2>
            <div class="right">
              <v-chip small class="caption my-2">
                {{ project.status }}
              </v-chip>
            </div>
          </v-flex>
        </v-layout>
        <v-divider></v-divider>
      </v-card>
    </v-container>
  </div>
</template>

<script>
import db from "@/fb";

export default {
  data() {
    return {
      projects: []
    };
  },
  methods: {
    sortBy(prop) {
      this.projects.sort((a, b) => (a[prop] < b[prop] ? -1 : 1));
    }
  },
  created() {
    db.collection("projects").onSnapshot(res => {
      const changes = res.docChanges();

      changes.forEach(change => {
        if (change.type === "added") {
          this.projects.push({
            ...change.doc.data(),
            id: change.doc.id
          });
        }
      });
    });
  }
};
</script>

<style>
/* border */
.project.complete {
  border-left: 4px solid green;
}
.project.ongoing {
  border-left: 4px solid orange;
}
.project.overdue {
  border-left: 4px solid tomato;
}
</style>
