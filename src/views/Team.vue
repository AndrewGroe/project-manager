<template>
  <div class="team">
    <h1 class="subheading">Team</h1>
    <v-container fill-height class="my-5">
      <v-layout row wrap>
        <v-flex xs12 sm6 md4 lg3 v-for="person in team" :key="person.name">
          <v-card class="text-center ma-3">
            <v-avatar size="100" class="white darken-2">
              <img :src="person.avatar" alt="User Avatar" />
            </v-avatar>
            <v-card-text>
              <div class="subheading">{{ person.name }}</div>
              <div>{{ person.role }}</div>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn>
                <v-icon small left>mdi-message</v-icon>
                <span>Message</span>
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      team: [
        { name: "Andrew", role: "Jack of all Trades", avatar: "/avatar-2.png" },
        {
          name: "Colleen",
          role: "Floral Firecracker",
          avatar: "/avatar-3.png"
        },
        { name: "Harley", role: "Trouble Maker", avatar: "/avatar-4.png" },
        { name: "Lux", role: "Master Manipulator", avatar: "/avatar-1.png" }
      ]
    };
  }
};
</script>
