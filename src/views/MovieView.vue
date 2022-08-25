<template>
   <div v-if="movies.length !== 0">
   <center>
   <el-table
    :data="movies"
    style="width: 50%">
    <el-table-column
      label="Release Date"
      width="180">
      <template slot-scope="scope">
        <i class="el-icon-time"></i>
        <span style="margin-left: 10px">{{ scope.row.DOR.slice(0,10) }}</span>
      </template>
    </el-table-column>
    <el-table-column
      label="Name"
      width="180">
      <template slot-scope="scope">
        <el-popover trigger="hover"  placement="left">
          <p>Name: {{ scope.row.name }}</p>
          <p>Directed By: {{ scope.row.Directedby }}</p>
          <div slot="reference" style="color:darkcyan">
            {{ scope.row.name }}
          </div>
        </el-popover>
      </template>
    </el-table-column>
    <el-table-column
      label="About">
      <template slot-scope="scope">
        <el-button
          size="mini"
          >
          <router-link class="link" :to="{name: 'MovieDetails' , params :{id : scope.row.id}}">Details
        </router-link>
      </el-button>
      </template>
    </el-table-column>
    <el-table-column
      label="Operations"
      width="180">
      <template slot-scope="scope">
        <el-button
          @click.native.prevent="removeItem(scope.row.id)"
          type="text"
          size="small">
          Remove          
          <i class="el-icon-delete"></i>
        </el-button>
        <el-button
          @click="handleUpdate(scope.row.id)"
          type="text"
          size="small"
          style="margin-left:20%;color:gold">
           Update
          <i class="el-icon-edit"></i>
        </el-button>
      </template>
    </el-table-column>
    </el-table>
    <br>
    <el-button
    @click="drawer = true" 
    type="primary" 
    style="margin-left: 16px;"> ADD
    </el-button>
    </center>
    <el-drawer
      title="Add New Movie"
      :visible.sync="drawer"
      direction="rtl"
      custom-class="demo-drawer"
      ref="drawer">
      <div class="demo-drawer__content">
      <el-form ref="form" :model="form" style="margin: 5% 2%" label-width="80px">
         <br>
          <el-form-item style="margin-left:5%" label="MovieName">
            <el-input style="width:70%" v-model="form.name"></el-input>
          </el-form-item>
          <el-form-item style="margin-left:5%" label="DirectorName">
            <el-input style="width:70%" v-model="form.Directedby"></el-input>
          </el-form-item>
          <el-form-item style="margin-left:5%" label="ReleaseDate">
              <el-col >
                <el-date-picker type="date" placeholder="Pick a Release date" v-model="form.Dor" style="width: 70%;"></el-date-picker>
              </el-col>
          </el-form-item>
          <el-form-item style="margin-left:5%" label="Budget">
            <el-input style="width:70%" v-model="form.budget"></el-input>
          </el-form-item>
          <el-form-item style="margin-left:5%" label="HeroName">
            <el-input style="width:70%" v-model="form.hero"></el-input>
          </el-form-item>
          <el-form-item style="margin-left:5%" label="HeroImageURL">
            <el-input style="width:70%" v-model="form.heroimg"></el-input>
          </el-form-item>
          <el-form-item style="margin-left:5%" label="HeroineName">
            <el-input style="width:70%" v-model="form.heroine"></el-input>
          </el-form-item>
          <el-form-item style="margin-left:5%" label="HeroineImageURL">
            <el-input style="width:70%" v-model="form.heroineimg"></el-input>
          </el-form-item>
          <el-form-item style="margin-left:5%" label="MovieImageURL">
            <el-input style="width:70%" v-model="form.img"></el-input>
          </el-form-item>
          <el-button
            @click="handleSubmit" 
            type="primary" 
            style="margin-left: 16px;"> Insert
          </el-button>
      </el-form>
      </div>
    </el-drawer>
   <el-drawer
        title="Make Your Update Here!"
        :visible.sync="dialog"
        direction="ltr"
        custom-class="demo-drawer"
        ref="drawer"
    >
        <div class="demo-drawer__content">
          <el-form ref="form" :model="form" style="margin: 5% 2%" label-width="80px">
         <br>
          <el-form-item style="margin-left:5%" label="MovieName">
            <el-input style="width:70%" v-model="movieNew.name">{{movieNew.name}}</el-input>
          </el-form-item>
           <el-form-item style="margin-left:5%" label="DirectorName">
            <el-input style="width:70%" v-model="movieNew.Directedby">{{movieNew.Directedby}}</el-input>
          </el-form-item>
          <el-form-item style="margin-left:5%" label="ReleaseDate">
              <el-col >
                <el-date-picker type="date" placeholder="Pick a Release date" v-model="movieNew.DOR" style="width: 70%;"></el-date-picker>
              </el-col>
          </el-form-item>
          <el-form-item style="margin-left:5%" label="Budget">
            <el-input style="width:70%" v-model="movieNew.budget"></el-input>
          </el-form-item>
          <div class="ac" v-for="(data , index) in movieNew.actor" v-bind:key="index">
            <el-form-item style="margin-left:5%" label="castName">
              <el-input style="width:70%" v-model="data.name"></el-input>
            </el-form-item>
             <el-form-item style="margin-left:5%" label="castImageUrl">
              <el-input style="width:70%" v-model="data.url"></el-input>
            </el-form-item>
          </div>
          <el-form-item style="margin-left:5%" label="MovieImageURL">
            <el-input style="width:70%" v-model="movieNew.img"></el-input>
          </el-form-item>
          <el-button
            @click="boughtItem(movieNew.id)" 
            type="primary" 
            style="margin-left: 16px;"> Update
          </el-button>
      </el-form>
        </div>
    </el-drawer> 
   </div>
   <div v-else>
      <div class="lds-hourglass"></div>
      <p>Loading Data....</p>
   </div> 
</template>
<script>
import axios from "axios";

export default{
    
    data(){
        return{
            movies:[],
            movieNew : {},
            drawer: false,
            dialog: false,
            form: {
                name: '',
                Dor : '',
                img : '',
                Directedby : '',
                budget : '',
                hero : '',
                heroimg : '',
                heroine : '',
                heroineimg: ''
            }
        }
    },
    created(){
        fetch('http://localhost:3000/movies')
         .then(res => res.json())
         .then(data => this.movies = data)
         .catch(err => console.log(err.message));
    },
    methods: {
      
      removeItem(id) {
            axios.delete(`http://localhost:3000/movies/${id}`)
            this.movies = this.movies.filter((movie) => movie.id !== id)
    },
      handleSubmit(){
          let project = {
            name: this.form.name,
            DOR: this.form.Dor,
            img: this.form.img,
            Directedby: this.form.Directedby,
            budget: this.form.budget,
            actor: {
                hero :{
                  name : this.form.hero,
                  url :  this.form.heroimg
                },
              heroine : {
                name : this.form.heroine,
                url : this.form.heroineimg
              }
            }
        }
        fetch('http://localhost:3000/movies', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json',
        'Accept': 'application/json'
         },
        body: JSON.stringify(project)
        })
        .then(() => this.$router.push('/movie'))
        .catch(err => console.log(err))
      },
    handleUpdate(id){
       console.log(id);
       fetch('http://localhost:3000/movies/'+id)
         .then(res => res.json())
         .then(data => this.movieNew = data)
         .catch(err => console.log(err.message));
         this.dialog = true;
    },
    async boughtItem(id) {
        fetch("http://localhost:3000/movies/"+id, 
        {
              headers: {
              Accept: "application/json",
              "Content-Type": "application/json"
            },
            method: "PUT",
            body: JSON.stringify(this.movieNew)
        })
        .then(function (response) {
          return response.json();
        })
        .then(function (data) {
          console.log(data);
        })
        .then(() => this.$router.push({name: 'MovieDetails' , params :{id : id}}))
        .catch(err => console.log(err.message));
        
      }
    }
}

</script>
<style>
@import url("//unpkg.com/element-ui@2.15.8/lib/theme-chalk/index.css");
    .link{
        text-decoration: none;
        color: black;
       
    }
    
    .ac{
        border: none;
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

</style>