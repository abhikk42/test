<template>
  <div class="aboutContainer" ref="details">
    <!-- Zoom image section -->
    <section class="modal" id='myModal'>
       <div class="close" @click="CloseModal">Close</div>
      
        
        <!-- <img :src="gallery[selectedind].image" class="modal-content" /> -->
         
   
           <VueSlickCarousel
         

            @afterChange="afterPageChange"
              cssEase='linear'
         
             @click="CloseModal"
             :style="{width:'100%',height:'100%'}"
            ref="fullviewcarousel"
          >
        
            <div v-for="(val,ind) in gallery" :key="ind"  >
              <img :src="val.image"   class="modal-content"  />
            </div>
            </VueSlickCarousel>

   
 
    </section>
      <!-- Zoom image section  ends-->


    <section class="head">
      <Header ref="header" />
    </section>


<!----------------------------------- PDP page actual code section ------------------------------------------ -->
    <section class="detailcontainer">
      <div class="topdetail" v-if="gallery.length > 0"
>
        <div class="topcol1">
          <VueSlickCarousel
            v-bind="topsettings"
            @afterChange="afterPageChange"
            ref="gallerycarousel"
          >
            <div v-for="(val,ind) in gallery" :key="ind" class="carouselimg" @click="handleFullView(ind)">
              <img :src="val.image" width="100%" height="100%" />
            </div>
          </VueSlickCarousel>
        </div>
        <div class="topcol2">
          <div
            v-for="(val, ind) in gallery"
            :key="ind"
            class="imgcont"
            @click="changeSlide(ind)"
          >
            <img :src="val.image" :class="activesmallimg==ind?'topimg activesmallimg':'topimg'" width="100%" height="100%" />
          </div>
        </div>

        <div class="topcol textcont">
          <h4 class="head">{{ data ? data.name : "" }}</h4>
          <h6 class="price" :style="{textTransform:'capitalize'}" >Rs.{{ data.selling_price }} <span :style="{color:'#FFCB05',fontSize:'15px',textTransform:'capitalize'}">Inclusive of all taxes</span></h6>
          <div class="mobprice">
            <h6 :style="{textTransform:'capitalize'}">Rs.{{ data.selling_price }} <span :style="{color:'#FFCB05',fontSize:'15px'}">Inclusive of all taxes</span></h6>
            <button class="btncart">ADD TO CART</button>
          </div>
          <div class="sizecont">
            <h6 :style="{fontSize:'15px'}">select size</h6>
            <div v-for="(val,ind) in size" :key="ind" :class="activesize==ind?'sizebox activesize':'sizebox'" @click="activesize=ind">{{ val }}</div>
          </div>
          <div class="colorcont">
            <h6>COLOURS</h6>
            <div
              v-for="(val, ind) in data.bestseller_products"
              :key="ind"
              :class="activecolor == ind ? 'dots dotsactive' : 'dots'"
              :style="{ background: val.color=='white'?'whitesmoke':val.color }"
              @click="handleActiveColor(ind)"
            ></div>
          </div>
          <div class="cartcont">
            <button class="btncart">ADD TO CART</button>
            <h5 class="wishlist">Add to wishlist</h5>
          </div>

          <div class="accordiancont">
            <div class="filterdiv">
              <button class="accordion" @click="toggleAccordian(0)">
                SIZE GUIDE
                <span class="icon"
                  ><font-awesome-icon
                    :icon="
                      accorddata[0].open == 'false' ? 'fa-plus' : 'fa-minus'
                    "
                  />
                </span>
              </button>
              <div v-if="accorddata[0].open == 'true'" class="filtercontent">
                Lorem ipsum dolor sit amet, consectetur adipisicing elit. Nulla,
                temporibus!
              </div>
            </div>
            <div class="filterdiv">
              <button class="accordion" @click="toggleAccordian(1)">
                FIT
                <span class="icon"
                  ><font-awesome-icon
                    :icon="
                      accorddata[1].open == 'false' ? 'fa-plus' : 'fa-minus'
                    "
                  />
                </span>
              </button>
              <div v-if="accorddata[1].open == 'true'" class="filtercontent">
                Lorem ipsum dolor sit amet consectetur adipisicing elit. Eos,
                fugit praesentium sequi cumque fugiat perferendis, maxime
                officia modi itaque unde quae enim pariatur voluptatem odio!
              </div>
            </div>
            <div class="filterdiv productacc">
              <button class="accordion" @click="toggleAccordian(2)">
                product description
                <span class="icon"
                  ><font-awesome-icon
                    :icon="
                      accorddata[2].open == 'false' ? 'fa-plus' : 'fa-minus'
                    "
                  />
                </span>
              </button>
              <div v-if="accorddata[2].open == 'true'" class="filtercontent">
                <ul>
                  <li v-for="val in data.visible_attributes" :key="val.code">
                    <h4>{{ val.code }}</h4>
                    {{ val.value }}
                  </li>
                </ul>
              </div>
            </div>
          </div>

          <!-- pincode information -->
          <div class="pincodecont">
            <h6>CHECK PINCODE FOR DELIVERY</h6>
            <div class="pinrow">
              <input type="text" placeholder="Enter pincode" class="pininput" />
              <button class="pinbtn">check</button>
            </div>
          </div>
        </div>
      </div>
     <!-- -------------Mid  row  Section --------------- -->
      <div class="midsection">
        <div class="imgcont">
          <img src="@/assets/fast.svg" alt="" />
          <h6>free shipping</h6>
        </div>
        <div class="imgcont">
          <img src="@/assets/quality.svg" alt="" />
          <h6>high quality unbeatable value</h6>
        </div>
        <div class="imgcont">
          <img src="@/assets/exchange.svg" alt="" />
          <h6>Easy Exchange</h6>
        </div>
      </div>

      <!---------------- Bottom Detail Section -------------->

      <div class="bottomcarousel">
        <h4>BEING HUMAN EXCLUSIVE</h4>

        <VueSlickCarousel
          v-bind="settings"
          class="vueslick"
          v-if="data.similar_products.length > 0"
        >
          <div class="slide" v-for="(val,ind) in data.similar_products" :key="ind" @click="handleSimilarProducts(val)">
            <img :src="val.image" />
            <h6 class="trend">/ IN TREND /</h6>
            <h6 class="slidedet name">{{ val.name }}</h6>
            <h6 class="slidedet price">Rs.{{ val.selling_price }}</h6>
          </div>
        </VueSlickCarousel>
      </div>
    </section>
<!----------------------------------- PDP page actual code section  ends------------------------------------------ -->

    <section>
      <Footer />
    </section>
  </div>
</template>
<script>
import VueSlickCarousel from "vue-slick-carousel";

import "vue-slick-carousel/dist/vue-slick-carousel.css";
import "vue-slick-carousel/dist/vue-slick-carousel-theme.css";
import axios from "axios";
import Footer from "../components/footer.vue";
// import Footer from "../components/footer.vue";

import Header from "../components/Header.vue";
export default {
  components: { VueSlickCarousel, Header, Footer },
  name: "detailpage",

  data() {
    return {
      api: "https://pim.wforwoman.com/pim/pimresponse.php/",
      service: "product",
      store: "1",
      gallery: [],
      url_key: "",
      selectedind:0,
      activesmallimg:0,
      activesize:0,
      activecolor: 0,
      sliderPageIndex: 0,

      accorddata: [{ open: "false" }, { open: "false" }, { open: "false" }],
      data: "",
      size: [],
      settings: {
        dots: false,
        focusOnSelect: true,
        arrows: true,

        infinite: true,
        autoplay: true,

        autoplaySpeed: 2000,
        vertical: false,
        slidesToShow: 3,
        slidesToScroll: 1,
      },
      topsettings: {
        dots: true,
        focusOnSelect: true,
        infinite: true,
        autoplay: true,
        autoplaySpeed: 4000,
        arrows: false,

        vertical: true,
        slidesToShow: 1,
        slidesToScroll: 1,
      },
    };
  },
  computed: {
    currentPage() {
      // always display 1 if page index is 0
      if (this.sliderPageIndex == 0) {
        return 1;
      } else {
        // compensate for slidesToScroll settings and compensate the shift by 1
        return this.sliderPageIndex / this.topsettings.slidesToScroll + 1;
      }
    },
  },

  mounted() {
    let key = this.$route.query.url_key;

    if (window.innerWidth < 768) {
      this.settings.slidesToShow = 2;
      // document.getElementsByClassName('slick-dots').style[1].display='none'
    }
    if (key) this.url_key = key;
    this.getdata();
  },
  methods: {
    changeSlide(ind) {
      this.$refs.gallerycarousel.goTo(ind);
      this.sliderPageIndex = ind;
      this.activesmallimg=ind;
    },
    handleSimilarProducts(val){
      this.$router.push({
       path: '/about',
        query: {
        url_key: val.url_key
        }
    }).catch(()=>{});
  this.$router.go()


    },
    CloseModal(){
        document.getElementById('myModal').style.display='none';

    },
    handleFullView(ind)
    {
      this.selectedind=ind;
        document.getElementById('myModal').style.display='block';
              this.$refs.fullviewcarousel.goTo(ind);

        // mymodal=mymodal=='block'?'none':'block'
    },
    afterPageChange(page) {
      let calc = (page / this.gallery.length - 1) % 100;
      // document.getElementsByClassName('slick-dots').style[0].backgroundSize=`${calc} +%100`;
      //     document.getElementsByClassName('slick-dots').style[0].attr=`aria-valuenow=${calc} );`;

      this.sliderPageIndex = page;
      this.activesmallimg=page;
    },
    async getdata() {
      let resp = await axios.get(this.api, {
        params: {
          service: this.service,
          store: 1,
          url_key: this.url_key,
        },
      });

      console.log("reponse", resp);

      if (resp.status == 200 || resp.status == 201) {
        this.data = resp.data.result;
        this.gallery = [...resp.data.result.gallery];
        let currval = this.data.size.replace(/[\])}[{(]/g, "");
        let size = currval.split(",");
        this.size = [...size];
      }
    },
    toggleAccordian(ind) {
      this.accorddata[ind].open =
        this.accorddata[ind].open == "true" ? "false" : "true";
    },
    handleActiveColor(ind) {
      this.activecolor = ind;
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Jost:ital,wght@0,100;0,300;0,400;0,500;0,600;0,700;1,200;1,400&family=Montserrat:ital,wght@0,100;0,400;0,500;1,100;1,400;1,500&family=Playfair+Display:ital,wght@0,400;0,500;0,700;1,400&display=swap");
@import url("https://db.onlinewebfonts.com/c/009a96399d6a238fd8a1d0be514c11ab?family=DK+Plakkaat");
*{
  min-height: 0;
min-width: 0;
}
.detailcontainer {
  
      background: #F8F8F8;

  width: 100%;
  
  text-transform: uppercase;
  font-family: "Montserrat";
  height: 100%;
}
/* Zoom Image css */

.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  padding-top: 20px;
left: 0;
  top: 0;
  border:none;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,1); /* Black w/ opacity */
}

/* Modal Content (Image) */
.modal-content {
  margin: auto;
  object-fit: contain;
  display: block;
  border:none;
 width: 100%;
 height: auto;
  max-width: 700px;
  max-height: 700px;
}



@keyframes zoom {
  from {transform:scale(0)}
  to {transform:scale(1)}
}

/* The Close Button */
.close {
  position: absolute;
  top: 15px;
  right: 35px;
  color: #f1f1f1;
  z-index: 999;
  font-size: 40px;
  font-weight: bold;
  transition: 0.3s;
}

.close:hover,
.close:focus {
  color: #bbb;
  text-decoration: none;
  cursor: pointer;
}

/* Zoom Image css ends */

/*---------------------------PDP page css-------------------*/
.topdetail {
  display: flex;
  height: 100%;


  overflow: hidden;

  font-family: "Montserrat";
  padding: 48px 50px;
}
.topdetail > .topcol1 {
  width: 40%;
  justify-content: center;

  height: 100%;
;
}
.topcol2 {
  width: 10%;
  display: flex;
  flex-direction: column;
}
.topcol2 .imgcont {
  width: 73px;
  cursor: pointer;
  
  margin: 0px 10px;
  margin-bottom:5px;
  height: 97px;
}
.topcol2 > .imgcont > img {
  height: 100%;
  width: 100%;
}
.imgcont img {
  width: 100%;
  
  height: 100%;
  margin: 5px 0px;
}

.textcont {
  flex-direction: column;
  justify-content: center !important;
  margin: 0px;
  overflow: hidden;

  width: 50%;
  padding: 0px 1%;
}
.colorcont {
  color: #747474;
  display: flex;
  padding: 20px 0px;
  flex-wrap: wrap;
  width: 100%;
}
.colorcont h6 {
  font-size: 15px !important;
  width: 100%;
  text-transform: uppercase;
}
.carouselimg {
  height: 100%;
  width: 100%;
}
.dots {
  width: 36px;
height: 36px;
  border-radius: 50%;
  cursor: pointer;
  margin: 0px 10px;
  display: inline-block;
}
.dotsactive {
  box-shadow: 0 0 0 2px white, 0 0 0 3px #707070;
}
.activesmallimg{
  border:1px solid black;
}

.textcont h4 {
  font-size: 25px;
  font-weight: 400;
  text-transform: uppercase;
}
.textcont h6 {
  font-size: 22px;
  margin: 10px auto;
  font-weight: 400;
}
img {
  height: 10%;
  width: 100%;
}
.sizecont {
  font-size: 15px;
  width: 100%;
  margin: auto 0px;

  display: flex;
  gap: 12px;
  flex-wrap: wrap;
  align-items: center;
  color: #747474;
}
.sizecont > h6 {
  width: 100%;
}
.topimg {
  height: 100%;
  width: 100%;
}
.cartcont {
  display: inline-flex;
  width: 100%;
  padding-top: 35px;

  align-items: center;
  justify-content: flex-start;
}
.wishlist {
  color: #231F20;
  font-size: 17px;
  text-decoration: underline;
  font-weight: 400;
  margin: 0px 10%;
}
.btncart {
  padding: 22px 130px;
  font-family: "Montserrat";
  font-size: 17px;
  background: #231f20;
  cursor: pointer;
  color: #ffcb05;
}
.sizebox {
  display: flex;
  align-items: center;
  font-size: 15px;
  cursor: pointer;
  justify-content: center;
  padding: 20px 25px;
  border: 1px solid #dfdfdf;
  align-items: center;
}
/*-----------Accordian---------*/
.accordiancont {
  padding: 50px 0px;
}
.accordion {
  color: #231f20;
  font-family: "Montserrat";
  cursor: pointer;
  text-transform: uppercase;
  padding: 15px 10px 15px 10px;
  width: 100%;
  border: none;
  text-align: left;

  font-weight: 500;
  outline: none;
  font-size: 15px;
  transition: 0.4s;
}
.accordion > .icon {
  float: right;
}
.topcol > .imgcont > .icon {
  float: right;
  font-size: 15px;
}
.filterdiv {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  width: 90%;
  margin: 0px 0px;
  color: #231f20;
  font-weight: 400;

  padding-top: 2px;
  padding-bottom: 2px;
  border-top: 1px solid #231F20;
}
.productacc {
  border-bottom: 1px solid #231f20;
}
/*--------pincode-----------*/
.pincodecont {
  display: inline-flex;
  width: 90%;
  flex-wrap: wrap;
}
.pinrow {
  display: flex;
  width: 70%;
}
.pincodecont h6 {
  font-size: 15px;
  margin: 10px 0px;
  font-weight: 500;
  color: #747474;
  font-family: "Montserrat";
}
.pininput {
  width: 70%;
  background: #f3f3f3;
  border: none;
  outline: none;

  padding: 22px 32px;
}
.pinbtn {
  color: #ffcb05;
  background: #231f20;
  padding: 20px 35px;
  font-size: 17px;
  outline: none;
  text-transform: uppercase;
  text-align: center;
}
.filtercontent {
  font-size: 14px;
  max-height: 300px;
  overflow-y:scroll;
  width: 100% !important;
}
li {
  list-style-type: none;
  display: inline-flex;
  width: 100% !important;
  font-size: 12px;
  justify-content: space-between;
}
li > h4 {
  font-size: 14px !important;
}

/* Mid container */
.midsection {
  display: flex;
  justify-content: space-around;
  border-top: 1px solid #f3f3f3;
  overflow: hidden;

  border-bottom: 1px solid #f3f3f3;

  padding: 10px 40px;
}

.imgcont > img {
  width: 40%;
  height: 40%;
}
.imgcont {
  display: inline-flex;
  width: 10%;
  text-transform: capitalize;
  height: 100px;
  flex-wrap: wrap;
  margin: 0px;

  justify-content: center;
  align-items: center;
  text-align: center;
  flex-direction: row;
}
.imgcont > h6 {
  font-size: 15px;
  color: #231f20;
  font-weight: 500;
}

/* Bottom container */
.bottomcarousel {
  overflow: hidden;

  height: 80%;
  display: flex;
  gap: 27px;

  justify-content: center;

  flex-wrap: wrap;
  margin: 0px 0px;

  padding: 80px 100px;
}
.bottomcarousel h4 {
  text-align: center;
  font-family: "DK Plakkaat";
  font-weight: 400;

  font-size: 45px;
}
.slide {
  display: flex;
  align-items: center;
  padding: 0px 5%;
  width: 448px;
  cursor: pointer;
height: 568px;

  justify-content: center !important;

  /* width: 100%;
  height: 100%; */
}
.slide img {
  width: 100%;
    mix-blend-mode: multiply;


  height: 100%;
}

.vueslick {
  width: 100%;
  color: black;
  padding-left: 20px;
  height: 100%;
  position: relative;
}
.slider {
  width: 100%;
}
.slidedet {
  font-size: 16px;
  font-family: "Monsterrat";

  font-weight: 400;
  margin: 0px;

  color: #202020;
}
.name {
  padding-bottom: 5px;
}

.activesize{
  background:green;
  color:white;
  
  }
.slidedet .price {
  font-size: 17px;
}
.trend {
  color: #747474;
  width: 90%;
  margin-bottom: 10px;
  font-size: 13px;
  font-weight: 400;

  padding: 17px 2px;

  border-bottom: 1px solid #747474;
}
/* button.slick-prev:before, button.slick-next:before {
      background-color: red !important;
} */
.mobprice {
  display: none;
}
.bottomcarousel > h4 {
  padding: 22px 50px;
}
@media (min-width: 769px) and (max-width: 1300px) {
  .btncart {
    padding: 11px 30px;
  }
}
@media (max-width: 768px) {
  .aboutContainer {
    overflow: hidden;
  }
  .detailContainer {
    padding: 0px !important;
    margin: 0px !important;
    overflow: hidden;

    width: 100%;
  }
  .topdetail {
    flex-direction: column;
    width: 100%;
    margin: 0px !important;
    padding: 0px 50px;
  }
  .topdetail > .topcol1 {
    width: 100%;
    height: 100%;
  }

  .topcol2 {
    display: none;
  }
  .accordiancont {
    padding: auto 0px;
    width: 100%;
  }
  .accordion {
    width: 100%;
  }
  .topcol {
    width: 100%;
  }
  .bottomcarousel {
    padding: 80px 50px;
    height: 100%;
    gap: 34px;
    width: 100%;
  }
  .bottomcarousel > h4 {
    padding: 50px 0px;
  }
  .textcont > .head {
    padding-top: 55px;
  }
  .slide {
    width: 100%;
    height: 100%;
  }
  
  .filterdiv {
    background: #f3f3f3;
    width: 100%;
  }
  .accordion {
    background: #f3f3f3;
  }

  .price {
    display: none;
  }
  .cartcont {
    display: none;
  }
  .mobprice {
    display: inline-flex;
    justify-content: space-between;
    background: #f3f3f3;
    padding: 20px 0px;
    flex-wrap: wrap;
    width: 100%;
  }


  .mobprice > h6 {
    text-align: left;
    margin: 10px 0px;
  }
  .dots {
    margin: 10px;
  }
  .pinrow {
    width: 100%;
  }
  .pininput {
    width: 75%;
  }
  .btncart {
    padding: 22px 50px;
  }
}
  /* 100% Image Width on Smaller Screens */
@media only screen and (max-width: 700px){
  .modal-content {
    width: 100%;
  }
}
</style>
