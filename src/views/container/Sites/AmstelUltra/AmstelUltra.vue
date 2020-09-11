<template>
  <v-container id="dashboard" fluid tag="section">
    <v-row>
      <v-col cols="12">
        <div
          class="font-weight-light mt-1"
          style="color:#3c4858; font-size: 25px"
        >
          Challenger Team
        </div>
      </v-col>
    </v-row>

    <v-row v-if="respData.length">
      <v-col v-for="item in respData" :key="item.id" sm="12" md="4">
        <base-material-card color="transparent" image hover-reveal>
          <template v-slot:image>
            <v-img :src="item.uri" height="400px" width="400px" />
          </template>

          <template v-slot:reveal-actions>
            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn class="mx-1" v-bind="attrs" icon v-on="on">
                  <v-icon>mdi-view-split-vertical</v-icon>
                </v-btn>
              </template>

              <span>View</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  class="mx-1"
                  color="success"
                  light
                  icon
                  v-on="on"
                >
                  <v-icon class="success--text">
                    mdi-pencil
                  </v-icon>
                </v-btn>
              </template>

              <span>Edit</span>
            </v-tooltip>

            <v-tooltip bottom>
              <template v-slot:activator="{ attrs, on }">
                <v-btn
                  v-bind="attrs"
                  class="mx-1"
                  color="error"
                  light
                  icon
                  v-on="on"
                >
                  <v-icon class="error--text">
                    mdi-close
                  </v-icon>
                </v-btn>
              </template>

              <span>Remove</span>
            </v-tooltip>
          </template>

          <v-card-title class="justify-center font-weight-light mb-2 mt-3">
            Publicacion
          </v-card-title>
          <v-file-input label="Imagen" filled prepend-icon="mdi-image" />
          <!-- <v-text-field v-model="title" label="Title" readonly /> -->
          <!-- <v-text-field v-model="data.subtitle" label="Subtitle" /> -->
          <v-row class="justify-center">
            <v-col sm="12" md="6"
              ><v-text-field
                v-model="item.like_count"
                label="Likes"
                type="number"
                prepend-icon="mdi-star"
                readonly
              ></v-text-field
            ></v-col>
            <v-col sm="12" md="6"
              ><v-text-field
                v-model="item.comments_count"
                label="Comentarios"
                type="number"
                prepend-icon="mdi-comment-text-multiple-outline"
                readonly
              ></v-text-field
            ></v-col>
          </v-row>
          <v-textarea
            v-model="item.caption"
            label="Caption"
            class="text-no-wrap"
            auto-grow
            rows="2"
            row-height="30"
            readonly
          ></v-textarea>
        </base-material-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  name: "AmstelUltra",

  data() {
    return {
      respData: [],
      title: null,
      data: {
        title: "Amstel Ultra",
        subtitle:
          "Es una bebida energética inspirada en el poder más fuerte de la naturaleza: EL SOL. Una bebida elaborada con ingredientes de origen natural que te dará esa energía que necesitas para vivir el día a día con mayor intensidad y alcanzar tu máximo rendimiento."
      },
      tabs: 0,
      list: {
        0: false,
        1: false,
        2: false
      }
    };
  },

  computed: {
    totalSales() {
      return this.sales.reduce((acc, val) => acc + val.salesInM, 0);
    }
  },

  mounted: async function() {
    const resp = await axios.get(
      "https://apim-sf-sitios-dev.azure-api.net/cms/amstel/api/feed?identifier=challenger-team"
    );
    this.title = resp.data.data.feed.name;
    this.respData = resp.data.data.feed.posts;
    console.log("data: ", this.respData);
  },

  methods: {
    complete(index) {
      this.list[index] = !this.list[index];
    }
  }
};
</script>
