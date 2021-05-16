<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">

      </v-col>
      <v-card
          class="mx-auto"
          max-width="500"
          elevation="9"
          min-width="500"
      >
        <v-toolbar
            color="teal lighten-1"
            dark
        >

          <v-app-bar-nav-icon @click="drawer=true"></v-app-bar-nav-icon>









          <v-toolbar-title>Fragebox</v-toolbar-title>
        </v-toolbar>

        <v-navigation-drawer v-model="drawer" temporary absolute>
          <v-list nav dense>
            <v-list-item-group active-class="teal--text text--lighten-4">
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-delete-outline</v-icon>
                </v-list-item-icon>
                <v-list-item-title @click="items=[]">Lösche alle Fragen</v-list-item-title>
              </v-list-item>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon>mdi-file-document-outline</v-icon>
                </v-list-item-icon>
                <v-list-item-title @click="linkToAnordnung">Aktuelle Verordnung</v-list-item-title>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-navigation-drawer>

        <v-list two-line elevation="2">
          <v-list-item-group
              active-class="teal--text"
              multiple
          >
            <template v-for="(item) in items">
              <v-list-item :key="item.question">
                <template v-slot:default="{ active }">
                  <v-list-item-content>

                    <v-textarea
                        v-if="!active"
                      readonly
                      v-text="item.question"></v-textarea>
                    <v-textarea
                        v-if="active"
                        readonly
                        v-text="texto">
                    </v-textarea>
                   <v-spacer></v-spacer>
                    <v-textarea
                        v-if="active"
                        readonly
                        v-text="item.link"></v-textarea>
                    <v-textarea
                        v-if="!active"
                        readonly
                        class="teal--text text--lighten-2"
                        v-text="item.answer"></v-textarea>

                  </v-list-item-content>

                  <v-list-item-action>
                    <v-list-item-action-text v-text="item.action"></v-list-item-action-text>

                    <v-tooltip bottom>
                      <template v-slot:activator="{ on, attrs }">

                    <v-icon
                        v-bind="attrs"
                        v-on="on"
                      v-if="item.accuracy==0"
                      color = "red">
                      mdi-cancel
                    </v-icon>
                      </template>
                      veraltete Information
                    </v-tooltip>


                    <v-tooltip bottom v-if="!active">
                      <template v-slot:activator="{ on, attrs }">

                    <v-icon
                        v-bind="attrs"
                        v-on="on"
                        v-if="!active && item.answer !='Stelle mir einfach eine Frage...'"
                        color="teal lighten-1"
                        @click="helpUs(item.answer)"
                    >
                      mdi-emoticon-frown-outline
                    </v-icon>
                      </template>
                      falsche Antwort
                    </v-tooltip>


                  </v-list-item-action>
                </template>
              </v-list-item>

            </template>
          </v-list-item-group>
          </v-list>


        <v-card-actions>
          <v-spacer v-if="loading"></v-spacer>
          <v-progress-circular
              v-if="loading"
              :size="50"
              color="green"
              indeterminate
          ></v-progress-circular>
          <v-spacer v-if="loading"></v-spacer>
          <v-text-field
              v-if="!loading"
              color="teal lighten-1"
              v-model="newQuestion"
              label="Stell eine Frage..."
              regular
              clearable
              prepend-inner-icon="mdi-forum"
          />
          <v-btn
              v-if="!loading"
              color="teal lighten-1"
              text
              v-on:keyup.enter="askQuestion"
              @click="askQuestion"
          >Senden
          </v-btn>
        </v-card-actions>

      </v-card>

    </v-row>
  </v-container>
</template>

<script>
import axios from "axios"
import Swal from "sweetalert2"
  export default {
    name: 'ChatCard',

    data: () => ({

      loading: false,
      texto: "Basierend auf:",
      drawer: false,
      newQuestion:"",
     items: [
       {
         question: "Hallo, wie kann ich dir helfen?",
         answer: "Stelle mir einfach eine Frage...",
         accuracy: 1, //wenn 0 dann nicht mehr aktuell
         link: "Das basiert auf nichts, das ist die Begrüßung!!",
         probability: 1,

       },
       {
         question: "Welche Maske muss der Gast tragen?",
         answer: "Eine normale Stoffmaske welche Mund und Nasenbereich bedeckt genügt.",
         accuracy: 0, //wenn 0 dann nicht mehr aktuell
         link: "test",
         probability: 1,

       },


     ],

    }),
    methods: {
      helpUs(answer){
        Swal.fire({
          icon: 'error',
          title: '<strong>Falsche Antwort?</strong>',
          text: 'Hilf uns besser zu werden!',
          footer: 'die Antwort "' +answer +'" wird für unsere Techniker markiert',

          showConfirmButton: true,
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Yes, do it!',

          width: 600,
          padding: '3em',
          backdrop: `
    rgba(0,0,123,0.4)
    url("https://sweetalert2.github.io/images/nyan-cat.gif")
    left top
    no-repeat
  `
        })
      },
      async askQuestion(){
        if(this.newQuestion!=""){
          await axios.post('http://127.0.0.1:5000/question',
              {
                question: this.newQuestion
              })
          .then(response => {this.items.push({question: this.newQuestion, answer: response.data.answer, accuracy: response.data.accuracy,probability: response.data.probability,link: response.data.link})}

          )
          .catch(err => {console.log(err)})

          this.newQuestion = ""

        }
      },
      linkToAnordnung() {
        window.open("https://www.ris.bka.gv.at/Dokumente/BgblAuth/BGBLA_2021_II_214/BGBLA_2021_II_214.html", "_blank");
      }


    },
  }
</script>


<style scoped>
.v-list{
  height:400px;
  overflow-y:auto
}
</style>