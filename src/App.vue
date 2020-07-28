<template>
  <v-app :class="{noselect: shiftKeyOn}">
    <v-main>
      <v-container>
        <v-card>
          <v-card-title>Shift + Click Bulk Select Demo</v-card-title>
          <v-data-table
            v-model="selectedRows"
            @current-items="current = $event"
            @item-selected="bulkSelect"
            :headers="headers"
            :items="desserts"
            item-key="id"
            class="elevation-1"
            show-select
            unselectable
          >
            <template slot="items" slot-scope="{item, selected}">
              <tr :active="selected" >
                <td>
                  <v-checkbox primary hide-details :input-value="selected"></v-checkbox>
                </td>
                <td class="text-xs-right">{{ item.id }}</td>
                <td>{{ item.name }}</td>
                <td class="text-xs-right">{{ item.calories }}</td>
                <td class="text-xs-right">{{ item.fat }}</td>
              </tr>
            </template>
          </v-data-table>
        </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { desserts } from "@/mocks/data.json";
export default {
  name: "App",
  components: {},
  created() {
    const self = this;
    self.keyDownHandler = function ({ key }) {
      if (key == "Shift") self.shiftKeyOn = true;
    };
    self.keyUpHandler = function ({ key }) {
      if (key == "Shift") self.shiftKeyOn = false;
    };
    window.addEventListener("keydown", this.keyDownHandler);
    window.addEventListener("keyup", this.keyUpHandler);
  },
  beforeDestroy() {
    window.removeEventListener("keydown", this.keyDownHandler);
    window.removeEventListener("keyup", this.keyUpHandler);
  },
  data() {
    return {
      shiftKeyOn: false,
      current: [],
      selectedRows: [],
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
      desserts,
    };
  },
  methods: {
    clickRow(item, { isSelected, select }) {
      select(!isSelected);
    },
    bulkSelect({item: b, value }) {
      const { selectedRows, current, shiftKeyOn } = this;

      if (selectedRows.length == 1 && value == true && shiftKeyOn) {
        const [a] = selectedRows;
        let start = current.findIndex((item) => item == a);
        let end = current.findIndex((item) => item == b);
        if (start - end > 0) {
          let temp = start;
          start = end;
          end = temp;
        }
        for (let i = start; i <= end; i++) {
          selectedRows.push(current[i]);
        }
      }
    },
  },
};
</script>
<style>
.noselect {
  user-select: none; /* CSS3 (little to no support) */
  -ms-user-select: none; /* IE 10+ */
  -moz-user-select: none; /* Gecko (Firefox) */
  -webkit-user-select: none;
}
</style>
