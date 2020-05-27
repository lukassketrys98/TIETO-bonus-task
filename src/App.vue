<template>
  <div id="app">
      
<div class="md-layout md-alignment-center">
        <div class="md-layout-item md-size-50 md-small-size-80">
          <md-card class = "card md-elevation-10">
              <md-toolbar :class="{ active: isActive }" class="md-primary md-elevation-10" >
                    <h3 class="md-title">{{title}}</h3>
                  </md-toolbar>
                <md-card-header>
                <div class="md-title intro-text">{{introText}}</div>
                </md-card-header>
                <div class="photo">
                  <img :src="avatar" class = "avataro" alt="">
                </div>
                <md-card-content>
                   <div class="md-layout-item md-alignment-center">
            </div>
                  <div class="facts">
                    <p id = "fact" v-for="fact in facts">{{fact}} </p>
                    
                    <div class="lds-hourglass ">
                    <p id ="loader-text">Loading...</p>

                    </div>
                  </div>
                </md-card-content>

                <md-card-actions>
                  <md-button :class="{ hideOnDogs: isActive }" class = "md-raised md-primary"  @click="showDogsWindow">HACK THE FACTS</md-button>
                 
                  
                </md-card-actions>
               
              </md-card>
             
        </div>
</div>
 
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      facts: null,
      avatar: 'https://cdn3.iconfinder.com/data/icons/avatars-9/145/Avatar_Cat-512.png',
      isActive: false,
      title: 'CATS FACTS',
      introText: 'Welcome to the cats facts library'
     
    }
  },
  mounted () {
    // REQUESTS FOR DATA FROM CATS API AND PUSHES IT INTO ARRAY
    const myRequest = new Request('https://cat-fact.herokuapp.com/facts');
    fetch(myRequest).then( response =>  response.json()).then( response => {
    
        var ats;
        for(let key in response){
          ats=response[key];
        }
        var newArray = []
        for(let i = 0;i < ats.length;i++){
          newArray.push(ats[i].text)
        }
        this.facts = newArray.slice(1, 16);
      });
  },

  methods: {
    // REPLACES ALL THE WORDS "CAT" WITH THE WORD "DOG"  
     replaceCats(){
      for(let i = 0; i < this.facts.length;i++){
        if(this.facts[i].includes("Cats") || this.facts[i].includes("cats") || this.facts[i].includes("Cat") || this.facts[i].includes("cat")){
          this.facts[i] = this.facts[i].replace("Cats","DOGS");
          this.facts[i] = this.facts[i].replace("cats","DOGS");
          this.facts[i] = this.facts[i].replace("Cat","DOG");
          this.facts[i] = this.facts[i].replace("cat","DOG");
        }
      }
        
    },
    // SHOW OR HIDE FACTS WHILE LOADING
    factsDisplay(displayMode){
        var catFacts = document.querySelectorAll('#fact');
        for(let i = 0; i < catFacts.length;i++){
           catFacts[i].style.display = displayMode;
           catFacts[i].style.borderBottom = "2px solid #f27979";
        }
    },
    // UPDATE CARD WITH NEW TITLE, DESCRIPTION AND AVATAR 
    updateInfo(){
         this.avatar = 'https://echouser.com/wp-content/uploads/2018/01/Frenchie.jpg';
         this.isActive = true;
         this.introText= 'All the cats facts has been infected by DOGS';
         this.title= 'DOG FACTS';
    },
    // DISPLAYS NEW WINDOW FOR DOGS 
    showDogsWindow(){
      this.factsDisplay('none');

      document.querySelector('.facts').style.minHeight = '700px';
      document.querySelector('.card').style.minHeight = '1000px';
      var loader = document.querySelector('.lds-hourglass');
      var loaderText = document.querySelector('#loader-text');

      loaderText.style.display = 'inline-block';
      loader.style.display = 'inline-block';
      
      this.replaceCats();
      
      setInterval( () =>{ 
         this.updateInfo();
         this.factsDisplay('block');
         loaderText.style.display = 'none';
         loader.style.display = 'none';
       }, 1500);
    }
  }
}
</script>

<style>

*{
  font-family: 'Play', sans-serif;
}
.card{
  margin: 20px 0;
}

img{
    border-radius: 50%;
    background-repeat: no-repeat;
    background-size: cover;
    width:100px;
    height:100px;
    object-fit: cover;
    margin-top: 15px;
    margin-bottom: 15px;
}

.photo{
  text-align: center;
}

.hideOnDogs{
  display:none;
}

.active{
  background: #f27979!important;
}

.intro-text{
  text-align: center;
}
.facts{
  text-align: center;
}

#fact{
  border-bottom: 2px solid #448aff;
  padding-top: 15px;
}

/* LOADER STYLE */

#loader-text{
  display: none;
  text-transform: uppercase;
  color:#448aff;
  font-weight: 800;
}
.lds-hourglass {
  display: none;
  position: relative;
  width: 80px;
  height: 80px;
  margin: 200px auto;

}

.lds-hourglass:after {
  content: " ";
  display: block;
  border-radius: 50%;
  width: 0;
  height: 0;
  margin: 8px;
  box-sizing: border-box;
  border: 32px solid #fff;
  border-color: #fff transparent #fff transparent;
  animation: lds-hourglass 1.2s infinite;
   background: #0853c9;
  
    
}
@keyframes lds-hourglass {
  0% {
    transform: rotate(0);
    animation-timing-function: cubic-bezier(0.55, 0.055, 0.675, 0.19);

  }
  50% {
    transform: rotate(900deg);
    animation-timing-function: cubic-bezier(0.215, 0.61, 0.355, 1);
  }
  100% {
    transform: rotate(1800deg);
  }
}
</style>
