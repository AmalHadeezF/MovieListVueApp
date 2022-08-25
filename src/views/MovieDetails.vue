<template>
    <div v-if="movie !== null">
        <img :src="movie.img">
            <div class="a">
                <p>Movie Name : <strong>{{movie.name}}</strong></p>
                <p>Relese Date : <strong>{{movie.DOR}}</strong></p>
                <p>Directed by : <strong>{{movie.Directedby}}</strong></p>
                <p>Budget : <strong>{{movie.budget}}</strong></p>
            <div class="ab">
                <p>Cast :</p>
                    <div class="ac" v-for="(data , index) in movie.actor" v-bind:key="index">
                    <img class="ab_img" :src="data.url">
                    <p><strong>{{data.name}}</strong></p>
                    </div>
                </div>
            </div>
            <router-link to="/movie">
            <button class="button"><span>Back </span></button>
            </router-link>
    </div>
    <div v-else>
        <div class="lds-hourglass"></div>
        <p>Loading Data....</p>
    </div>

</template>

<script>
export default {
    props : ['id'],
    data(){
        return{
            movie : null
        }
    },
    mounted(){
        fetch('http://localhost:3000/movies/'+this.id)
         .then(res => res.json())
         .then(data => this.movie = data)
         .catch(err => console.log(err.message))
    },
    methods: {
      goBack() {
        this.$router.go(-1)
      }
    }
}
</script>
<style>
img {
  border-radius: 10%;
  width : 30%;
  margin-right: 5%;
  float: left;
}
.a{
    width: auto;
    text-align: left;
    padding: 5%;
}
.ab{
    display: flex;
    margin-bottom: 1%;
}
.ab_img{
  vertical-align: middle;
  width: 100px;
  height: 100px;
  border-radius: 50%;
}
.ac{
    margin: 2%;
}
 .lds-hourglass {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-hourglass:after {
  content: " ";
  display: block;
  border-radius: 50%;
  width: 0;
  height: 0;
  margin: 8px;
  box-sizing: border-box;
  border: 32px solid rgb(0, 0, 0);
  border-color: rgb(0, 0, 0) transparent rgb(0, 0, 0) transparent;
  animation: lds-hourglass 1.2s infinite;
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
.button {
  border-radius: 4px;
  background-color: #0c9500;
  border: none;
  color: #FFFFFF;
  text-align: center;
  font-size: 20px;
  padding: 10px;
  width: 150px;
  transition: all 0.5s;
  cursor: pointer;
  margin: 5px;
}

.button span {
  cursor: pointer;
  display: inline-block;
  position: relative;
  transition: 0.5s;
}

.button span:after {
  content: '\00bb';
  position: absolute;
  opacity: 0;
  top: 0;
  left: 20px;
  transition: 0.8s;
  transform: rotate(3.142rad);
}

.button:hover span {
  padding-left: 25px;
}

.button:hover span:after {
  opacity: 1;
  left: 0;
}
</style>