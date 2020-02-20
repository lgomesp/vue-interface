<template>
  <div id="main-app" class="container">
    <div class="row justify-content-center">
      <appointment-list :appointments="appointments" @remove="removeItem" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import _ from "lodash";
import AppointmentList from "./components/AppointmentList";

export default {
  name: "MainApp",
  data: function() {
    return {
      appointments: [],
      aptIndex: 0
    };
  },
  components: {
    AppointmentList
  },
  mounted() {
    axios
      .get("./data/appointments.js")
      .then(response => (this.appointments = response.data.map(item => {
        item.aptId = this.aptIndex;
        this.aptIndex++;
        return item
      })));
  },
  methods: {
    removeItem: function(appointment) {
      this.appointments = _.without(this.appointments, appointment);
    }
  }
};
</script>