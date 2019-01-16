<template>
  <div id="app">
    <div class="container-app">
      <!-- <div class="menubar">
        <img class="brand-logo" :src="img" alt="">
        <div class="align-center">
          <a class="align-center" href="#">link</a>
        </div>
      </div> -->
      <div class="action-principal" v-show="true">
        <div class="flex-center" style="margin-bottom:10px;">
          <label 
            class="flex-center c-pointer button-capture-image">
            <i>
              <svg style="width:60px;height:60px" viewBox="0 0 24 24">
                <path fill="#6065fd" d="M4,4H7L9,2H15L17,4H20A2,2 0 0,1 22,6V18A2,2 0 0,1 20,20H4A2,2 0 0,1 2,18V6A2,2 0 0,1 4,4M12,7A5,5 0 0,0 7,12A5,5 0 0,0 12,17A5,5 0 0,0 17,12A5,5 0 0,0 12,7M12,9A3,3 0 0,1 15,12A3,3 0 0,1 12,15A3,3 0 0,1 9,12A3,3 0 0,1 12,9Z" />
              </svg>
            </i>
            <input
              v-show="false"
              type="file"
              name="image"
              accept="image/*"
              capture="camera"
              @change="setImage"/>
          </label>
        </div>
        <span class="msg_action">
          "Tomale una foto a la placa para obtener información de la unidad."
        </span>
      </div>


      <div class="container-info" v-show="true">
        <vue-cropper
          ref="cropper"
          :src="imgSrc"
          alt="Source Image"
          :cropmove="cropImage"
          :min-container-width="370"
          :min-container-height="300"
          :view-mode="2"
          :drag-mode="'move'"
          :image-smoothing-quality="'low'"
          :img-style="{ 'width': '300px', }"
        >
        </vue-cropper>
        <canvas id="canvas1" width="300" height="500" v-show="false"></canvas>
        <!-- <img :src="cropImg" style="width: 200px;border: 1px solid gray" alt="Cropped Image" /> -->
        <button @click="cropImage">Generate</button>
      </div>
    </div>
    <div>

    </div>
  </div>
</template>

<script>
import VueCropper from 'vue-cropperjs';


export default {
  name: 'app',
  data() {
    return{
      img:require('./assets/logo.png'),
      imgSrc: '',
      cropImg: ''
    }
  },
  methods: {
    cropImage: function() {
      // get image data for post processing, e.g. upload or setting image src
        this.cropImg = this.$refs.cropper.getCroppedCanvas().toDataURL();
    },
    setImage: function(e) {
      let file = e.target.files[0];
      if (!file.type.includes('image/')) {
        alert('Please select an image file');
        return;
      }
      if (typeof FileReader === 'function') {
        let reader = new FileReader();
        reader.onload = (event) => {
          this.resizeImage(event.target.result).then((response) => {
            this.imgSrc = response
            // rebuild cropperjs with the updated source
            this.$refs.cropper.replace(response);
          })
        };
        reader.readAsDataURL(file);
      } else {
        alert('Sorry, FileReader API not supported');
      }
    },
    resizeImage: function(img64) {
        return new Promise(function(resolve, reject) {
          let canvas = document.getElementById('canvas1')
          let ctx = canvas.getContext('2d')
          let image = new Image()
          image.onload = function() {
            ctx.drawImage(image, 0, 0, 300, 500);
            resolve(canvas.toDataURL());
          }
          image.src = img64
        })

      
    }
    

  },
  components: {
		VueCropper
	},
}
</script>

<style>
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background: #2c3e50;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.container-app{
  max-width: 400px;
  width: 100vw;
  height: 100vh;
  max-height: 700px;
  background: #FB57B8;
  background: linear-gradient(to bottom, rgb(255, 112, 196) 0%, rgb(255, 60, 177) 100%);
  justify-content: center;
  display: flex;
  flex-direction: column;
}
.menubar{
  background: blue;
  display: flex;
  justify-content: space-between;
  padding: .5rem 1rem;

}
.brand-logo{
  width: 50px;
  height: 50px;
}
.align-center{
  display: flex;
  align-items: center;
}
.flex-center{
  display: flex;
  justify-content: center;
  align-items: center;
}
.c-pointer{
  cursor: pointer;
}
.button-capture-image{
  background: #e8e9ff;
  line-height: 0;
  padding: 1rem;
  border-radius: 50%;
}
.container-info{
  background: #FFF;
  width: 95%;
  /* height: 230px; */
  align-self: center;
  border-radius: 4px 4px 0 0;
}
.action-principal{
  align-self: center;
  width: 90%;
}
.msg_action{
  color:white;
  padding-top: 10px;
}
.c1{
  position: absolute;
  z-index: 2;
  border: dashed 2px red;
  box-sizing: border-box;
  top: 70px;
  left: 50px;
}
</style>
