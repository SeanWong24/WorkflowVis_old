<template>
  <ion-app>
    <ion-header>
      <ion-toolbar>
        <ion-title>{{title}}</ion-title>
        <ion-buttons slot="end">
          <ion-button @click="upload()">
            <ion-icon slot="icon-only" name="cloud-upload" size="large"></ion-icon>
            <input
              type="file"
              id="file-upload-input"
              accept=".json"
              @change="fileUploaded()"
              ref="fileUploadInput"
            />
          </ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>
    <ion-content fixed id="main-content">
      <ion-grid>
        <ion-row>
          <ion-col size="9">
            <VisView />
          </ion-col>
          <ion-col size="3">
            <ion-card>
              <ControlView />
            </ion-card>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-app>
</template>


<script>
import VisView from "./VisView";
import ControlView from "./ControlView";
import { addIcons } from "ionicons";
import { cloudUpload } from "ionicons/icons";

addIcons({
  "md-cloud-upload": cloudUpload.md,
  "ios-cloud-upload": cloudUpload.ios
});

export default {
  name: "MainView",
  props: {
    title: String
  },
  components: {
    VisView,
    ControlView
  },
  computed: {
    fileData: {
      get: function() {
        return this.$fileData;
      },
      set: function(value) {
        this.$fileData = value;
        // eslint-disable-next-line
        console.log(this.fileData);
      }
    }
  },
  methods: {
    upload() {
      this.$refs.fileUploadInput.click();
    },
    fileUploaded() {
      const file = this.$refs.fileUploadInput.files[0];
      if (file && file.name.split(".").pop() === "json") {
        const fileReader = new FileReader();
        fileReader.onload = () => {
          const fileContent = fileReader.result;
          this.fileData = JSON.parse(fileContent);
        };
        fileReader.readAsText(file);
      } else {
        this.fileData = null;
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#main-content {
  height: calc(100% - 56px);
  margin-top: 56px;
}

ion-grid {
  height: 100%;
  padding: 0;
}

ion-row {
  height: 100%;
}

ion-card {
  height: 100%;
  margin: 0;
}

#file-upload-input {
  display: none;
}
</style>
