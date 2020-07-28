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
          ></v-data-table>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
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
      desserts: [
        {
          id: 1,
          name: "Frozen Yogurt",
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0,
          iron: "1%",
        },
        {
          id: 2,
          name: "Ice cream sandwich",
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3,
          iron: "1%",
        },
        {
          id: 3,
          name: "Eclair",
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0,
          iron: "7%",
        },
        {
          id: 4,
          name: "Cupcake",
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3,
          iron: "8%",
        },
        {
          id: 5,
          name: "Gingerbread",
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9,
          iron: "16%",
        },
        {
          id: 6,
          name: "Jelly bean",
          calories: 375,
          fat: 0.0,
          carbs: 94,
          protein: 0.0,
          iron: "0%",
        },
        {
          id: 7,
          name: "Lollipop",
          calories: 392,
          fat: 0.2,
          carbs: 98,
          protein: 0,
          iron: "2%",
        },
        {
          id: 8,
          name: "Honeycomb",
          calories: 408,
          fat: 3.2,
          carbs: 87,
          protein: 6.5,
          iron: "45%",
        },
        {
          id: 9,
          name: "Donut",
          calories: 452,
          fat: 25.0,
          carbs: 51,
          protein: 4.9,
          iron: "22%",
        },
        {
          id: 10,
          name: "KitKat",
          calories: 518,
          fat: 26.0,
          carbs: 65,
          protein: 7,
          iron: "6%",
        },
      ],
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
