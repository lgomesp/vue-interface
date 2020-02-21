<template>
  <div id="main-app" class="container">
    <div class="row justify-content-center">
      <add-apointment @add="addItem" />
      <search-appointment
        @searchRecords="searchAppointments"
        :myKey="filterKey"
        :myDir="filterDir"
        @requestKey="changeKey"
        @requestDir="changeDir"
      />
      <appointment-list :appointments="filteredAppointments" @remove="removeItem" @edit="editItem" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import _ from "lodash";
import AppointmentList from "./components/AppointmentList";
import AddApointment from "./components/AddAppointment";
import SearchAppointment from "./components/SearchAppointment";

export default {
  name: "MainApp",
  data: function() {
    return {
      appointments: [],
      aptIndex: 0,
      searchTerms: "",
      filterKey: "petName",
      filterDir: "asc"
    };
  },
  components: {
    AppointmentList,
    AddApointment,
    SearchAppointment
  },
  mounted() {
    axios.get("./data/appointments.js").then(
      response =>
        (this.appointments = response.data.map(item => {
          item.aptId = this.aptIndex;
          this.aptIndex++;
          return item;
        }))
    );
  },
  computed: {
    searchedApts: function() {
      return this.appointments.filter(item => {
        return (
          item.petName.toLowerCase().match(this.searchTerms.toLowerCase()) ||
          item.petOwner.toLowerCase().match(this.searchTerms.toLowerCase()) ||
          item.aptNotes.toLowerCase().match(this.searchTerms.toLowerCase())
        );
      });
    },
    filteredAppointments: function() {
      return _.orderBy(
        this.searchedApts,
        item => {
          return item[this.filterKey].toLowerCase();
        },
        this.filterDir
      );
    }
  },
  methods: {
    removeItem: function(appointment) {
      this.appointments = _.without(this.appointments, appointment);
    },
    editItem: function(id, field, text) {
      const aptIndex = _.findIndex(this.appointments, {
        aptId: id
      });
      this.appointments[aptIndex][field] = text;
    },
    addItem: function(apt) {
      apt.aptId = this.aptIndex;
      this.aptIndex++;
      this.appointments.push(apt);
    },
    searchAppointments: function(terms) {
      this.searchTerms = terms;
    },
    changeKey: function(term) {
      this.filterKey = term;
    },
    changeDir: function(term) {
      this.filterDir = term;
    }
  }
};
</script>