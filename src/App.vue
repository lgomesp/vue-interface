<template>
  <div id="main-app" class="container">
    <h4>{{ title }}</h4>
    <font-awesome-icon icon="plus" class="mr-2" />Add Appointment
    <appointment-list :appointments="appointments" />
  </div>
</template>

<script>
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import axios from "axios";

import AppointmentList from "./components/AppointmentList";

export default {
  name: "MainApp",
  data: function() {
    return {
      title: "Appointment List",
      appointments: []
    };
  },
  components: {
    FontAwesomeIcon,
    AppointmentList
  },
  mounted() {
    axios
      .get("./data/appointments.js")
      .then(response => (this.appointments = response.data));
  }
};
</script>