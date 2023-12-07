<template>
  <v-data-table
    dense
    v-model:items="filteredmyDatas"
    :headers="headers"
    :items="myDatas"
    :item-value="name"
    class="elevation-1"
    :search="search"
    :rows-per-page-items="[25, 50]"
    @update:pagination="paginate"

  >
    <template v-slot:top>
      <!-- v-container, v-col and v-row are just for decoration purposes. -->
      <v-container fluid>
        <v-row>
          <v-col cols="6">
            <v-row class="pa-6">
              <!-- Filter for name name-->
              <v-text-field
                v-model="nameFilterValue"
                type="text"
                label="Name"
              ></v-text-field>
            </v-row>
          </v-col>

          <v-col cols="6">
            <v-row class="pa-6">
              <!-- Filter for calories -->
              <v-select
                :items="serviceList"
                v-model="serviceFilterValue"
                label="Services"
              ></v-select>
            </v-row>

            <v-row class="pa-6">
              <v-select
                :items="typeList"
                v-model="typesFilterValue"
                label="Types"
              ></v-select>
            </v-row>
          </v-col>
        </v-row>
      </v-container>
    </template>
  </v-data-table>
</template>

<script>
// Table info.
import data from "./data.json";

export default {
  data() {
    return {
      listSize: [ 25, 50],

      serviceList: [],
      typeList: [],

      // Filter models.
      nameFilterValue: "",
      serviceFilterValue: null,
      typesFilterValue: null,

      // Table data.
      myDatas: data,
    };
  },

  created() {
     this.myDatas = this.removeDuplicates(data);

    // Move the initialization to the created hook
    this.serviceList = Array.from(
      new Set(this.myDatas.map((item) => item.service))
    );
    this.typeList = Array.from(new Set(this.myDatas.map((item) => item.type)));
  },

  computed: {
    headers() {
      return [
        {
          title: "Name",
          align: "center",
          key: "name",
          filter: this.nameFilter,
          value: "name",
          sortable: true,
        },
        {
          title: "Service",
          text: "Services",
          value: "service",
          align: "center",
          filter: this.servicesFilter,
          sortable: true,
        },
        {
          title: "Type",
          align: "center",
          text: "Types",
          value: "type",
          filter: this.typesFilter,
          sortable: true,
        },
      ];
    },
    filteredmyDatas: {
      get() {
        return this.myDatas.filter((item) => {
          return (
            this.nameFilter(item.name) &&
            this.servicesFilter(item.service) &&
            this.typesFilter(item.type)
          );
        });
      },
      set(value) {
        this.$emit("update:filteredmyDatas", value);
      },
    },
  },
  methods: {
    removeDuplicates(array) {
      const seen = new Set();
      return array.filter((item) => {
        const itemString = JSON.stringify(item); // Convert the object to a string
        if (!seen.has(itemString)) {
          seen.add(itemString);
          return true;``
        }
        return false;
      });
    }, 

    nameFilter(value) {
      if (!this.nameFilterValue) {
        return true;
      }

      return value.toLowerCase().includes(this.nameFilterValue.toLowerCase());
    },

    servicesFilter(value) {
      if (this.serviceFilterValue === null) {
        return true;
      }

      return value === this.serviceFilterValue;
    },

    typesFilter(value) {
      if (!this.typesFilterValue) {
        return true;
      }

      return value === this.typesFilterValue;
    },
  },
};
</script>
