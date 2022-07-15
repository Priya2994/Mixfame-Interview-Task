<style scoped>
:deep() .v-data-table > .v-data-table__wrapper > table thead > tr > th {
  font-size: 16px;
  color: rgba(0, 0, 0, 0.7);
  text-transform: uppercase;
}
:deep() .v-data-table > .v-data-table__wrapper > table thead > tr > th:hover {
  color: rgba(0, 0, 0, 1) !important;
}
:deep() .v-text-field {
  max-width: 400px;
}
.v-application .elevation-1 {
  --table-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1),
    0 2px 4px -1px rgba(0, 0, 0, 0.06) !important;
  box-shadow: var(--table-ring-offset-shadow, 0 0 #0000),
    var(--table-ring-shadow, 0 0 #0000), var(--table-shadow) !important;
  border-radius: 10px;
}

.v-card__title {
  font-size: 22px;
  font-weight: 800;
}
.user-table table tr th {
  font-size: 14px !important;
}
</style>

<template>
  <div class="user-table">
    <v-data-table
      :headers="headers"
      :items="tabledatas"
      :search="search"
      item-key="name"
      class="elevation-1"
    >
      <template v-slot:[`top`]>
        <v-card-title>
          Users List
          <v-spacer></v-spacer>
          <v-text-field
            v-model="name"
            append-icon="mdi-magnify"
            label="Search by Username"
          ></v-text-field>

          <AddUser @onAddUser="onAddUser" /> </v-card-title
      ></template>
      <template v-slot:[`item.action`]="{ item }">
        <v-icon medium @click="deleteItem(item)" color="error">
          mdi-delete
        </v-icon>
      </template>
      <template v-slot:[`item.website`]="{ item }">
        <div @click="redirectTo(item)">
          {{ item.website }}
        </div>
      </template>
    </v-data-table>

    <template>
      <v-snackbar v-model="snack" :timeout="3000" :color="snackColor">
        {{ snackText }}

        <template v-slot:action="{ attrs }">
          <v-btn v-bind="attrs" text @click="snack = false"> Close </v-btn>
        </template>
      </v-snackbar>
    </template>
  </div>
</template>

<script>
import axios from "axios";
import AddUser from "./AddUser";
export default {
  name: "table-list-row",
  components: {
    AddUser,
  },
  mounted: function () {
    this.getTableList();
  },
  data: function () {
    return {
      message: "Table List Row",
      tabledatas: [],
      search: "",
      name: "",
      snack: false,
      snackColor: "",
      snackText: "",
      defaultItem: {
        name: "",
        email: "",
        address: "",
        phone: "",
        website: "",
        companyname: "",
      },
    };
  },
  computed: {
    headers() {
      return [
        {
          text: "Username",
          value: "name",
          filter: (f) => {
            return (f + "").toLowerCase().includes(this["name"].toLowerCase());
          },
        },
        { text: "Email", value: "email" },
        { text: "Address", value: "modifiedAddress" },
        { text: "Phone", value: "phone" },
        { text: "Website", value: "website" },
        { text: "Company Name", value: "modifiedCompName" },
        { text: "Actions", value: "action", sortable: false },
      ];
    },
  },
  methods: {
    getTableList: function () {
      const url = "https://jsonplaceholder.typicode.com/users";
      axios
        .get(url, {
          dataType: "json",
          headers: {
            Accept: "application/json",
            "Content-Type": "application/json",
          },
          mode: "no-cors",
          credentials: "include",
        })
        .then(({ data }) => {
          if (data.length) {
            this.tabledatas = this.modifyResponse(data);
          }
        })
        .catch(function (error) {
          console.log(error);
        });
    },

    modifyResponse(data) {
      let tableData = data.length === 0 ? [] : [...data];
      return tableData.map((value) => {
        value[
          "modifiedAddress"
        ] = `${value.address.suite}, ${value.address.street}, ${value.address.city},${value.address.zipcode}`;
        value["modifiedCompName"] = `${value.company.name}`;
        return value;
      });
    },

    // add table row
    onAddUser(item) {
      this.tabledatas.unshift(item);
      this.tabledatas = this.modifyResponse(this.tabledatas);
      this.snack = true;
      this.snackColor = "success";
      this.snackText = "Added New User List Successfully";
    },
    // delete table row
    deleteItem(item) {
      const index = this.tabledatas.indexOf(item);
      confirm("Are you sure you want to delete this user?") &&
        this.tabledatas.splice(index, 1);
      //tried jsonplaceholder delete api its giving success status but not giving the new user list
      // axios.delete(`https://jsonplaceholder.typicode.com/users/${item}`)
      // .then((response) => console.log(response.status))
      // .catch((response) => console.log(response.status));
    },
    redirectTo(item) {
      console.log('https://'+item.website, "item ===")
      if (item.website) {
        const host =['http://','https://'];
        const url = item.website.includes(host[0]) || item.website.includes(host[1]) ? item.website : host[1] + item.website;
        window.open(url, "_blank");
      }
    },
  },
};
</script>
