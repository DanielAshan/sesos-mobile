<template>
  <Page class="page">
    <ActionBar class="action-bar">
      <Label class="action-bar-title" text="Home"></Label>
    </ActionBar>

    <StackLayout>
      <Label class="info" horizontalAlignment="center" verticalAlignment="center">
        <FormattedString>
          <Span class="fa" text.decode="&#xf135; " />
          <Span :text="message" />
        </FormattedString>
      </Label>
      <Button text="Rejestracja" @tap="goToRegisterPage"/>
      <Button text="Potwierdź obecność" @tap="goToLecturePage"/> 
    </StackLayout>
  </Page>
</template>

<script>
import axios from "axios";
import Register from './Register.vue';
import Lecture from './Lecture.vue';
export default {  
  data() {
    return {
    };
  },
  computed: {
    message() {
      return "Witaj w SESOS";
    }
  },
  mounted() {
    // var Nfc = require("nativescript-nfc").Nfc;
    // // instantiate the plugin
    // var nfc = new Nfc();
    // nfc.available().then(function(avail) {
    //   console.log(avail ? "Yes" : "No");
    // });
    // nfc
    //   .setOnTagDiscoveredListener(function(data) {
    //     console.log(data.id);
    //     let hex = [];
    //     for (let x = 0; x < 4; x++) {
    //       hex.push(parseInt(data.id[x], 10).toString(16));
    //     }
    //     console.log(hex);
    //     console.log("Discovered a tag with ID " + data.id);
    //   })
    //   .then(function() {
    //     console.log("OnTagDiscovered listener added");
    //   });

    // console.log("Mounted");
  },
  methods: {
    goToRegisterPage() {
      console.log('Going to register');
      this.$navigateTo(Register);
    },
    goToLecturePage() {
      console.log('Going to lecture');
      this.$navigateTo(Lecture);
    }
  }
};
</script>

<style scoped lang="scss">
// Start custom common variables
@import "../app-variables";
// End custom common variables

// Custom styles
.fa {
  color: $accent-dark;
}

.info {
  font-size: 42;
}
</style>
