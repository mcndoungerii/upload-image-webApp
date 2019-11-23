<template>
  <div class="hello">
    <input style="display: none" type="file" @change="onFileSelected" ref="fileInput" />
    <button @click="$refs.fileInput.click()">Pick File</button>

    <button @click="onUpload">Upload</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "HelloWorld",

  data() {
    return {
      selectedFile: null
    };
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      console.log(this.selectedFile);
    },
    onUpload() {
      const fd = new FormData(); //this is a JS method which converts any file to binary data which can be stored in the backend.
      fd.append("image", this.selectedFile, this.selectedFile.name);
      axios
        .post(
          "https://us-central1-fir-cloud-functions-fc5d6.cloudfunctions.net/upload",
          fd,
          {
            onUploadProgress: uploadEvent => {
              console.log(
                "Upload Progress:" +
                  Math.round(uploadEvent.loaded / uploadEvent.total) * 100 +
                  "%"
              );
            }
          }
        )
        .then(res => console.log(res));
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
