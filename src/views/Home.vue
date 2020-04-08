<template>
  <div class="ion-page">
    <ion-header>
      <ion-toolbar>
        <ion-title>ZipInfo</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content class="ion-padding">
      <ZipSearch @get-zip="getZipInfo"></ZipSearch>
      <ZipInfo :info="info"></ZipInfo>
      <ClearInfo :info="info" @clear-info="clearInfo"></ClearInfo>
    </ion-content>
  </div>
</template>

<script>
import ZipSearch from "@/components/ZipSearch";
import ZipInfo from "@/components/ZipInfo";
import ClearInfo from "@/components/ClearInfo";

export default {
  name: "Home",
  components: {
    ZipSearch,
    ZipInfo,
    ClearInfo
  },
  data() {
    return {
      info: null
    };
  },
  methods: {
    getZipInfo(zip) {
      this.$axios
        .get(`https://api.zippopotam.us/us/${zip}`)
        .then(response => {
          if (response.status === 404) {
            this.showAlert();
          } else {
            this.info = response.data;
          }
        })
        .catch(err => {
          console.log(err.response);
        });
    },
    showAlert() {
      return this.$ionic.alertController
        .create({
          header: "Invalid Zipcode",
          message: "Please enter a valid US zipcode",
          buttons: ["OK"]
        })
        .then(a => a.present());
    },
    clearInfo() {
      this.info = null;
    }
  }
};
</script>
