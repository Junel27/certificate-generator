<template>
  <v-container fill-height>
    <v-row class="justify-center">
      <div class="center text-center">
        <h2>Generate Certificates</h2>
        <v-btn @click="generateUser()"
          color="primary">Create</v-btn>
      </div>
    </v-row>
  </v-container>
</template>

<script>
import { jsPDF } from "jspdf";

export default {
  name: "Home",
  data: () => ({
    users: [
      {
        first_name : 'Gusion',
        middle_name : 'Assassin',
        last_name : 'Blade',
        suffix : ''
      },
      {
        first_name : 'Guinevere',
        middle_name : 'Fighter',
        last_name : 'Mage',
        suffix : ''
      },
    ]
  }),
  methods: {
    generateUser() {
      for(let x of this.users) {
        this.generatePDF(x)
      }
    },
    generatePDF(user) {  
      var getBase64ImageUrl = function(url, callback, mine) {
        var img = new Image();
        url = url.replace("http://","//");
        img.setAttribute('crossOrigin', 'anonymous');
        img.onload = function () {
          var canvas = document.createElement("canvas");
          canvas.width = this.width;
          canvas.height = this.height;
          var ctx = canvas.getContext("2d");
          ctx.drawImage(this, 0, 0);
          var dataURL = canvas.toDataURL(mine || "image/jpeg");
          callback(dataURL);
        };
        img.src = url;

        img.onerror = function(){

          console.log('on error')
          callback('');

        } 
      }   

      getBase64ImageUrl('final_certificate2.jpg', function(img) {
        const doc = new jsPDF({
          orientation: "landscape",
          unit: "in",
          format: "letter"
        });

        let width = doc.internal.pageSize.getWidth()
        let height = doc.internal.pageSize.getHeight()

        doc.output('datauri');
        doc.addImage(img, 'JPEG', 0, 0, width, height);  

        const fName = user.first_name.toUpperCase()
        const mName = user.middle_name
        const lName = user.last_name.toUpperCase()
        const sfx = user.suffix

        doc
        .setFont("MercuryTextG1Roman")
        .setFontSize(26)
        .setTextColor(0, 0, 0)
        .text(`${fName} ${mName != null ? mName.charAt(0).toUpperCase() +'.': ''} ${lName} ${sfx}, MD`, width / 2 + 0.43, 4.24, 'center')
        .save(`PAPP_certificate.pdf`);
      }); 
    },
  }
};
</script>
