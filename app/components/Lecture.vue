<template>
  <Page class="page">
    <ActionBar class="action-bar">
      <Label class="action-bar-title" text="Home"></Label>
    </ActionBar>

    <StackLayout v-if="!recordAdded">
      <Label class="info" horizontalAlignment="center" verticalAlignment="center">
        <FormattedString>
          <Span class="fa" text.decode="&#xf135; " />
          <Span :text="classroomText" />>
        </FormattedString>
      </Label>
      <Label class="info" horizontalAlignment="center" verticalAlignment="center">
        <FormattedString>
          <Span class="fa" text.decode="&#xf135; " />
          <Span :text="lectureNameText" />>
        </FormattedString>
      </Label>
      <Label class="info" horizontalAlignment="center" verticalAlignment="center">
        <FormattedString>
          <Span class="fa" text.decode="&#xf135; " />
          <Span :text="lectureStartTimeText" />>
        </FormattedString>
      </Label>
      <Label class="info" horizontalAlignment="center" verticalAlignment="center">
        <FormattedString>
          <Span class="fa" text.decode="&#xf135; " />
          <Span :text="lectureEndTimeText" />>
        </FormattedString>
      </Label>
    </StackLayout>
    <StackLayout v-if="recordAdded">
      <Label class="info" horizontalAlignment="center" verticalAlignment="center">
        <FormattedString>
          <Span class="fa" text.decode="&#xf135; " />
          <Span text="DODANO WPIS OBECNOŚCI" />
        </FormattedString>
      </Label>
    </StackLayout>
  </Page>
</template>

<script>
import axios from "axios";
export default {
  name: "Lecture",
  data() {
    return {
      recordAdded: false,
      classroom: "E3",
      lectureName: "Brak zajęć",
      lectureStartTime: "",
      lectureEndTime: "",
      list_id: ""
    };
  },
  computed: {
    classroomText() {
      return "Sala: " + this.classroom;
    },
    lectureNameText() {
      return "Zajęcia: " + this.lectureName;
    },
    lectureStartTimeText() {
      return "Początek zajęć: " + this.lectureStartTime;
    },
    lectureEndTimeText() {
      return "Koniec zajęć: " + this.lectureEndTime;
    }
  },
  mounted() {
    this.getCurrentLecture();
    var vm = this;
    var Nfc = require("nativescript-nfc").Nfc;
    // instantiate the plugin
    var nfc = new Nfc();
    nfc.available().then(function(avail) {
      console.log(avail ? "Yes" : "No");
    });
    nfc
      .setOnTagDiscoveredListener(function(data) {
        if (vm.list_id)  {

        }
        console.log("Discovered a tag with ID " + data.id);
      })
      .then(function() {
        console.log("OnTagDiscovered listener added");
      });

    console.log("Mounted");
  },
  methods: {
    getCurrentLecture() {
      console.log("Requesting lecture");
      axios
        .get("http://185.243.52.8/api/getCurrentLecture/E3")
        .then(response => {
          console.log("Response");
          if (Array.isArray(response.data) && response.data.length) {
            console.log('data');
            this.lectureName = response.data[0].lesson.name;
            this.lectureStartTime = response.data[0].start_date;
            this.lectureEndTime = response.data[0].end_date;
          } else {
            console.log('no data');
            this.lectureName = 'Brak zajęć';
            this.lectureStartTime = '';
            this.lectureEndTime = '';
            this.list_id =  '';
          }
        });
    },
    addAttendanceRecord() {
      axios.get("");
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
