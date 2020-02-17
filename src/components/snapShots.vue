<template>
  <div id="snapshoptView">
    <input
      type="button"
      id="fname"
      name="fname"
      @click="find()"
      value="Page SnapShot"
      class=" btn btn-outline-danger my-2 my-sm-0"
    />
  </div>
</template>
<script>
export default {
  name: "snapShot",
  data() {
    return {
      output: null
    };
  },
  props: ["eleID", "eleTag", "iframeTag"],
  methods: {
    find() {
      if (this.iframeTag) {
        let iframes = document.getElementsByTagName("iframe");
        iframes.forEach(iframe => {
          let snapshots = iframe.contentWindow.document.getElementsByTagName(
            "html"
          );
          this.createSnapShots(snapshots[0]);
          return true;
        });
      } else if (this.eleID) {
        let element = document.getElementById(this.eleID);
        this.createSnapShots(element);
        return true;
      } else {
        let element = document.getElementById(this.eleTag);
        this.createSnapShots(element);
        return true;
      }
      return false;
    },
    async createSnapShots(el) {
      console.log(el);
      const options = {
        type: "dataURL"
      };
      this.output = await this.$html2canvas(el, options);
      this.downloadSnapShots();
      return true;
    },
    downloadSnapShots() {
      let link = document.createElement("a");
      link.href = this.output;
      link.target = "_blank";
      link.download = "download";
      document.body.appendChild(link);
      link.click();
      link.remove();
      return true;
    }
  }
};
</script>
