<template>
  <Page class="page">
    <ActionBar class="action-bar">
      <Label class="action-bar-title" text="Home"></Label>
    </ActionBar>

    <StackLayout :class="{ registerComplete: !readyToRegister}">
      <Label class="info" horizontalAlignment="center" verticalAlignment="center">
        <FormattedString>
          <Span class="fa" text.decode="&#xf135; " />
          <Span :text="message" />
        </FormattedString>
      </Label>
    </StackLayout>
  </Page>
</template>

<script>
import axios from "axios";
export default {
  name: "Register",
  data() {
    return {
      apiPath: "/register/",
      readyToRegister: true,
      registrationNumber: "",
      messageCore: "Przyłóż NFC żeby się zarejestrować",
    };
  },
  computed: {
    message() {
      let message = this.messageCore + this.registrationNumber
      console.log(message); 
      return message;
    }
  },
  mounted() {
    var vm = this;
    var Nfc = require("nativescript-nfc").Nfc;
    // instantiate the plugin
    var nfc = new Nfc();
    nfc.available().then(function(avail) {
      console.log(avail ? "Yes" : "No");
    });
    nfc
      .setOnTagDiscoveredListener(function(data) {
        // console.log(data.id);
        // let hex = [];
        // for (let x = 0; x < 4; x++) {
        //   hex.push(parseInt(data.id[x], 10).toString(16));
        // }
        // console.log(hex);
        console.log("Discovered a tag with ID " + data.id);
        vm.sendRegisterRequest("123789963741");
      })
      .then(function() {
        console.log("OnTagDiscovered listener added");
      });
    console.log("Mounted");
  },
  methods: {
    sendRegisterRequest(nfc_id) {
      var vm = this;
      console.log('Request')
      this.messageCore = "Rejestruję!";
      axios
        .post("http://185.243.52.8/api" + this.apiPath + nfc_id)
        .then(response => {
          console.log('Response')
          this.messageCore = "Zarejestrowano - twój numer ID to: ";
          this.registrationNumber = response.data;
          this.readyToRegister = false;
          setTimeout(() => {
            console.log('timeout');
            vm.messageCore = "Przyłóż NFC żeby się zarejestrować";
            vm.registrationNumber = "";
            vm.readyToRegister = true;
          },2000)
        }, error => {
          console.log(error);
        });
    }
  }
};
</script>

<style scoped lang="scss">
// Start custom common variables
@import "../app-variables";
// End custom common variables

.registerComplete {
  background-color: green;
}
// Custom styles
.fa {
  color: $accent-dark;
}

.info {
  font-size: 52;
}
</style>
