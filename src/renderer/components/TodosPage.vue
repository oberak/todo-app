<template>
  <div class="text-xs-center">
    <h1>TODOs</h1>
    <v-btn
      color="red lighten-2"
      @click="dialog = true"
      dark
    >
      ADD
    </v-btn>

    <ul>
      <li v-for="item in items" :key="item">
        {{ item }}
      </li>
    </ul>
    <v-dialog
      v-model="dialog"
      width="500"
      persistent
    >
      <v-card>
        <v-card-title
          class="headline grey lighten-2"
          primary-title
        >
          Add TODO
        </v-card-title>

        <v-divider></v-divider>

        <v-card-text>
          <v-container grid-list-md>
            <v-layout wrap>
              <v-flex xs12>
                <v-text-field label="TODO" required v-model="work"></v-text-field>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            flat
            @click="dialog = false"
          >
            Cancel
          </v-btn>
          <v-btn
            color="primary"
            flat
            @click="save()"
          >
            SAVE
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        dialog: false,
        work: '',
        items: [],
      }
    },
    mounted() {
      this.$electron.ipcRenderer.on('todo-list', (event, data) => {
        this.items = data
      })
    },
    methods: {
      save() {
        this.$electron.ipcRenderer.send('add-todo', this.work)
        this.dialog = false
      },
    },
  }
</script>