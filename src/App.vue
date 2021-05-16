<template>
  <v-app>
    <v-app-bar
      app
      color="teal"
      dark
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fwww.publicdomainpictures.net%2Fpictures%2F320000%2Fvelka%2Fcoronavirus-infection-illustration.png&f=1&nofb=1"
          transition="scale-transition"
          width="40"
        />

        <h1>CovBrain</h1>
      </div>

      <v-spacer></v-spacer>

      <v-btn
          v-if="cards==1"
        @click="cards=0"
        target="_blank"
        text
      >
        <span class="mr-2">Admin Tools</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
      <v-btn
          v-if="cards==0"
          @click="cards=1"
          target="_blank"
          text
      >
        <span class="mr-2">ChatBot</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>

      <ChatCard v-if="cards==1"/>
      <MenuCards v-if="cards==0"/>
    </v-main>
  </v-app>
</template>

<script>
import ChatCard from './components/ChatCard';
import MenuCards from "@/components/MenuCards";

export default {
  name: 'App',

  components: {
    MenuCards,
    ChatCard,
  },

  data: () => ({
    cards:1,
    //
  }),
};
</script>
