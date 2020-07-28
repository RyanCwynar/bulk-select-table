<template>
  <v-app>
    <v-main>
      <v-container>
        <v-card>
          <v-data-table
            v-model="selected"
            @current-items="current = $event"
            @click.native="bulkSelect"
            @click:row="clickRow"
            :headers="headers"
            :items="desserts"
            item-key="id"
            class="elevation-1"
            show-select
          ></v-data-table>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import {desserts} from "@/mocks/data.json";
export default {
  name: "App",
  components: {},
  data() {
    return {
      current: [],
      selected: [],
      headers: [
        { text: "ID", value: "id" },
        {
          text: "Dessert",
          align: "left",
          sortable: false,
          value: "name",
        },
        { text: "Calories", value: "calories" },
        { text: "Fat (g)", value: "fat" },
      ],
      desserts: desserts,
    };
  },
  methods: {
    toggle(isSelected, select) {
      select(!isSelected);
    },
    clickRow(item, {isSelected, select}) {
      select(!isSelected)
    },
    bulkSelect(e){
      const { selected, current } = this
      const shiftOn = e.shiftKey
      // a perfect storm for bulk selection
      if(selected.length == 2 && shiftOn){
        const [a , b] = selected
        let start = current.findIndex(item => item == a)
        let end = current.findIndex(item => item == b)
        if(start - end > 0){
          let temp = start
          start = end
          end = temp
        }
        for(let i = start + 1; i < end; i++){
          selected.push(current[i])
        }
      }
    }

  },
};
</script>
