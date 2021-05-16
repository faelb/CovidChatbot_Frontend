<template>

  <v-container>
    <v-row>
      <AdminTools
          v-for="Menu in Menutiles"
          v-bind:key="Menu.id"
          v-bind:card-title="Menu.Title"
          v-bind:card-sub-title="Menu.Subtitle"
          v-bind:card-text="Menu.CardText"
          v-bind:link="Menu.link"
      >
      </AdminTools>
    </v-row>
  </v-container>

</template>

<script>
import AdminTools from "@/components/AdminTools";
export default {
  name: "MenuCards",
  data: () => ({
    Menutiles: [
      {id:0, Title: "Verordnung vergleichen", Subtitle: "*prüfe Gültigkeit der genutzten Verordnung", CardText: "jetzt prüfen!", link: "MenuProfile"},
      {id:1, Title: "Antworten anpassen", Subtitle: "*passe die KI an die aktuellen Verordnung an", CardText: "jetzt anpassen!", link: "MenuTool"},
      {id:2, Title: "Falsche Antworten prüfen", Subtitle: "*siehe Antworten die vom User falsch markiert wurden", CardText: "jetzt prüfen!", link: "MenuRequests"},
      {id:3, Title: "Statistiken", Subtitle: "*Siehe Nutzungsstatistiken", CardText: "jetzt anzeigen!", link: "MenuSearch"}

    ]
  }),
  components: {
    AdminTools,
  }
}
</script>

<style scoped>

</style>