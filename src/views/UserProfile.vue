<template>
    <div>
      <div class="card">
        <div class="head">
          <div class="circle"></div>
          <div class="img">
            <img :src="require('@/assets/ash.jpg')" alt="">
          </div>
        </div>
        <div class="description">
          <h3>{{ userProfile.name }}</h3>
          <h4>Entrenador pokemón</h4>
          <p>Lorem ipsum dolor sit amet consectetur, adipisicing elit. Eligendi, repellendus.</p>
        </div>
        <div class="contact">
          <router-link to="/">
            <a href="#">Regresar</a>
          </router-link>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        userProfile: {
          name: 'Rigo Iván',
          avatar: 'URL de la imagen del avatar',
        },
      };
    },
  };
  </script>
  
  <style scoped>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Baloo 2', cursive;
  }
  
  body {
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    background: #efefef;
    flex-wrap: wrap;
  }
  
  .card {
    position: relative;
    width: 300px;
    height: 400px;
    background: #fff;
    border-radius: 5px;
    overflow: hidden;
    transition: .5s;
    margin: 115px auto 100px auto; 
  }
  
  .card:hover {
    box-shadow: 0 5px 15px rgba(3, 89, 92, .5);
    transform: translateY(-15px);
  }
  
  .card .head {
    height: 125px;
    width: 100%;
    position: relative;
  }
  
  .card .head .circle {
    position: absolute;
    width: 300px;
    height: 300px;
    border-radius: 50%;
    background: #021222;
    bottom: 0;
  }
  
  .card .head .img {
    width: 150px;
    height: 150px;
    position: absolute;
    background: #fff;
    padding: 5px;
    border-radius: 50%;
    bottom: -30%;
    left: 50%;
    transform: translateX(-50%); 
  }
  
  .card .head .img img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    object-fit: cover;
  }
  
  .card .description {
    height: 200px;
    padding: 40px 20px 20px 20px;
    border-bottom: solid 1px rgba(6, 74, 76, .18);
    text-align: center;
  }
  
  .card .description h3,
  .card .description h4 {
    color: #021222;
  }
  
  .card .description p {
    margin-top: 20px;
    font-size: 15px;
  }
  
  .card .contact {
    width: 100%;
    height: 75px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .card .contact a {
    text-decoration: none;
    color: #fff;
    background: #021222;
    padding: 5px 20px;
    border-radius: 5px;
    transition: .3s;
  }
  
  .card .contact a:hover {
    background: #012627;
  }
  </style>
  