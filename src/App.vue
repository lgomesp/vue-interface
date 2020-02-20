<template>
  <div id="main-app" class="container">
    <div class="row justify-content-center">
      <add-apointment @add="addItem" />
      <appointment-list :appointments="appointments" @remove="removeItem" @edit="editItem" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import _ from "lodash";
import AppointmentList from "./components/AppointmentList";
import AddApointment from "./components/AddAppointment";

export default {
  name: "MainApp",
  data: function() {
    return {
      appointments: [],
      aptIndex: 0
    };
  },
  components: {
    AppointmentList,
    AddApointment
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
    }
  }
};
</script>