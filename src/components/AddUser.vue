<template>
  <v-dialog v-model="dialog" max-width="500px">
    <template v-slot:activator="{ on }">
      <div class="m-4">
        <v-btn color="success" class="ml-5" v-on="on" large> Add User</v-btn>
      </div>
    </template>
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-card class="px-3 py-3">
        <v-card-title>
          <span class="headline pl-3">{{ formTitle }}</span>
        </v-card-title>

        <v-card-text>
          <v-container>
            <v-row>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="user.name"
                  :rules="nameRules"
                  label="UserName*"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="user.email"
                  :rules="emailRules"
                  label="E-mail*"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="user.address.suite"
                  label="Suite*"
                  :rules="suiteRules"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="user.address.street"
                  label="Street*"
                  :rules="streetRules"
                  required
                ></v-text-field>
              </v-col>

              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="user.address.city"
                  label="City*"
                  :rules="cityRules"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="user.address.zipcode"
                  label="Zipcode*"
                  :rules="zipcodeRules"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="user.phone"
                  label="Phone Number*"
                  :rules="phoneNoRules"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="user.website"
                  label="Website *"
                  :rules="websiteRules"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" md="6">
                <v-text-field
                  v-model="user.company.name"
                  label="Company Name*"
                  :rules="companyNameRules"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn @click="close">Cancel</v-btn>
          <v-btn
            color="success"
            :disabled="
              !valid ||
              !user.name ||
              !user.email ||
              !user.address.suite ||
              !user.address.street ||
              !user.address.city ||
              !user.address.zipcode ||
              !user.phone ||
              !user.website ||
              !user.company.name
            "
            @click="save"
            >Save</v-btn
          >
        </v-card-actions>
      </v-card>
    </v-form>
  </v-dialog>
</template>

<script>
import axios from "axios";
export default {
  name: "add-edit-user",
  mounted: function () {
    this.user = {
      name: "",
      email: "",
      phone: "",
      website: "",
      address: {
        suite: "",
        street: "",
        city: "",
        zipcode: "",
      },
      company: {
        name: "",
      },
    };
  },
  data: () => ({
    dialog: false,
    valid: true,
    nameRules: [
      (v) => !!v || "Name is required",
      (v) => (v && v.length <= 10) || "Name must be less than 10 characters",
    ],
    cityRules: [(v) => !!v || "City is required"],
    emailRules: [
      (v) => !!v || "E-mail is required",
      (v) => /.+@.+\..+/.test(v) || "E-mail must be valid",
    ],
    suiteRules: [(v) => !!v || "Suite Name is required"],
    streetRules: [(v) => !!v || "Street Name is required"],
    zipcodeRules: [
      (v) => !!v || "ZipCode is required",
      (v) => (v && v.length <= 6) || "ZipCode must be less than 6 characters",
      (v) => /^[0-9]\d*$/.test(v) || "ZipCode must be valid digit",
    ],
    phoneNoRules: [
      (v) => !!v || "Phone Number is required",
      (v) =>
        (v && v.length <= 10) || "Phone Number must be less than 10 digits",
      (v) => /^[1-9]\d*$/.test(v) || "Phone Number must be valid digit",
    ],
    websiteRules: [(v) => !!v || "Website Field is required"],
    companyNameRules: [(v) => !!v || "Company Name is required"],
    user: {
      name: "",
      suite: "",
      email: "",
      phone: "",
      website: "",
      address: {
        suite: "",
        street: "",
        city: "",
        zipcode: "",
      },
      company: {
        name: "",
      },
    },
  }),
  computed: {
    formTitle() {
      return this.user && this.user.id ? "Edit Item" : "Add New User";
    },
  },
  watch: {
    dialog(val) {
      val || this.close();
    },
  },
  methods: {
    addUser() {
      const url = "https://jsonplaceholder.typicode.com/users";
      axios
        .post(url, this.user)
        .then(({ data }) => {
          if (data.id) {
            this.close();
            this.$emit("onAddUser", data);
            this.$refs.form.reset();
          }
        })
        .catch(() => {
          console.alert("Unable to add new user");
        });
    },
    close() {
      this.dialog = false;
      this.$refs.form.reset();
    },
    save() {
      this.$refs.form.validate();
      this.addUser();
    },
  },
};
</script>
