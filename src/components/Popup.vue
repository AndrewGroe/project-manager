<template>
  <v-layout justify-center>
    <v-dialog
      v-model="dialog"
      fullscreen
      hide-overlay
      transition="dialog-bottom-transition"
    >
      <template v-slot:activator="{ on }">
        <v-btn color="primary" dark v-on="on">
          <v-icon left>mdi-plus-box</v-icon><span>New Project</span>
        </v-btn>
      </template>
      <v-card>
        <v-toolbar dark color="primary">
          <v-btn icon dark @click="close">
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title>New Project</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
            <v-btn dark text @click="submit">Save</v-btn>
          </v-toolbar-items>
        </v-toolbar>

        <!-- form -->
        <v-card-text>
          <v-form class="px-3" ref="form">
            <v-text-field
              :rules="inputRules"
              v-model="title"
              label="Title"
              prepend-icon="mdi-folder"
            ></v-text-field>
            <v-textarea
              :rules="inputRules"
              v-model="content"
              label="Information"
              prepend-icon="mdi-pencil"
            ></v-textarea>
            <v-menu
              ref="menu"
              v-model="menu"
              transition="scale-transition"
              offset-y
              full-width
              min-width="290px"
            >
              <template v-slot:activator="{ on }">
                <v-text-field
                  :rules="inputRules"
                  :value="formattedDate"
                  label="Due date"
                  prepend-icon="mdi-calendar"
                  clearable
                  readonly
                  v-on="on"
                ></v-text-field>
              </template>
              <v-date-picker v-model="due" no-title scrollable>
                <v-spacer></v-spacer>
              </v-date-picker>
            </v-menu>

            <v-spacer></v-spacer>
            <v-btn :loading="loading" @click="submit" class="success mx-0 mt-3"
              >Add Project</v-btn
            >
          </v-form>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-layout>
</template>

<script>
import format from "date-fns/format";
import db from "@/fb";

export default {
  data() {
    return {
      dialog: false,
      title: "",
      content: "",
      due: null,
      menu: false,
      inputRules: [
        v => !!v || "This field is required",
        v => v.length >= 3 || "Minimum length is 3 characters"
      ],
      loading: false
    };
  },
  methods: {
    submit() {
      if (this.$refs.form.validate()) {
        this.loading = true;

        const project = {
          title: this.title,
          content: this.content,
          due: format(this.due, "Do MMM YYYY"),
          person: "Andrew",
          status: "ongoing"
        };
        db.collection("projects")
          .add(project)
          .then(() => {
            this.loading = false;
            this.dialog = false;
            this.$emit("projectAdded");
            close();
          });
      }
    },
    close() {
      this.loading = false;
      this.dialog = false;
      this.due = null;
      this.title = "";
      this.content = "";
    }
  },
  computed: {
    formattedDate() {
      return this.due ? format(this.due, "Do MMM YYYY") : "";
    }
  }
};
</script>
