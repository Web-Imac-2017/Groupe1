<template lang="html">
  <div id="home">
    <div class="angled_container angled_container--open-right">
      <md-layout :md-gutter="8" md-align="center">
        <md-layout md-flex-xsmall="100" md-flex-small="50" md-flex-medium="33">
          <md-whiteframe md-elevation="5" id="connect"><connection></connection></md-whiteframe>
        </md-layout>
        <md-layout md-flex-xsmall="100" md-flex-small="50" md-flex-medium="33">
          <description id="description" class="home-top-element"></description>
        </md-layout>
      </md-layout>
      <div class="firstInscription">
        <h2>Pas encore inscrit(e) ?</h2>
        <a href="#inscriptionForm"><md-icon>keyboard_arrow_down</md-icon></a>
      </div>
      <!--<md-layout md-flex-xsmall="100" md-flex-medium="50" md-flex-xlarge="25" md-align="center">
        <md-whiteframe id="register">
          <inscription></inscription>
        </md-whiteframe>
      </md-layout>-->
    </div>
    <md-whiteframe md-elevation="5" id="formulaire"><inscription></inscription></md-whiteframe>
  </div>
</template>

<script>
import connection from './Connection.vue'
import description from './Home-description.vue'
import inscription from './Inscription.vue'


export default {
  name: 'home',
  components: {
    connection,
    description,
    inscription
  }
}
</script>

<style lang="css" scoped>
#home {
  text-align: center;
}

#register {
 padding: 5vw;
 text-align: center;
 margin: 0 auto;
 background-color: white;
}

#description {
  width: 590px;
  position: relative;
  left: 15%;
  top: 80%;
  text-align: left;
  color: white;
}

#connect {
  position: relative;
  left: 25%;
  top: 20%;
  background-color: white;
  padding: 40px 30px;
  text-align: center;
}

.firstInscription {
  position: relative;
  top: 25%;
}

#formulaire {
	width: 500px;
	padding: 40px 40px;
	margin: 50px auto;
}

/*.switchButton {
	position: fixed;
	top: 25%;
	right: 10%;
  	z-index: 1;
}*/


/*********************** ANIMATION CSS ********************/


  /*background-image: url('../../assets/bg_test.png');
  background-repeat:no-repeat;
  -webkit-background-size: cover;
  background-size: cover;*/


/*a[ id= "formulaire" ]:target ~ #formulaire {
    -webkit-transform: translateY( 500px);
    transform: translateY( 500px );
}*/

/*********************** BACKGROUND CSS ********************/

.angled_container {
  height: 850px;
  position: relative;
  overflow: hidden;
  background-color: #fff;
}

.angled_container:before,
.angled_container:after {
  content: "";
  width: 100%; height: 100%;
  display: block;
  position: absolute;
  bottom: 0; right: 0;
}

.angled_container:before {
  background-color: #078b9c ;
  transform: skewY(30deg);
  transform-origin: right bottom;
  box-shadow: 0 16px 10px -13px rgba(0, 0, 0, 0.5);
}

.angled_container:after {
  background-color: #226986;
  background-image: url('../assets/arbre_bleu.jpg');
  background-size: 100% 100%;
  transform: skewY(-30deg);
  transform-origin: left bottom;
  box-shadow: 1px 1px 10px 0.5px rgba(0, 0, 0, 0.3);
}

</style>
