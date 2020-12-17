<template>
  <div id="app">
    <div class="header">
      <router-link to="/">
        <div class="logo">
          <img class="marshmallow" src="../../data/negative-space-logo.png" />
        </div>
      </router-link>
      <div class="title">
        <h1>Marshmallow Marketing Mail</h1>
      </div>
    </div>
    <div class="content">
      <router-view />
    </div>
    <div class="footer">
      <div class="link-container">
        <router-link to="/sendmessage" class="link">Send Message</router-link>
        <router-link to="/admin" class="link">Employee Portal</router-link>
      </div>
    </div>
  </div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: "Montserrat", sans-serif;
  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}

.button {
  width: 70%;
  margin: 30px auto 10px auto;
  background-color: #f48051;
  border: none;
  color: white;
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  font-size: 20px;
  padding: 10px 0px 10px 0px;
}


/* Header */
.header {
  display: flex;
  justify-content: center;
  padding: 10px 100px 0px 100px;
  background-color: #f48051;
  color: #ffffff;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
}

.marshmallow {
  width: 60px;
}

.title {
  margin-top: 5px;
}

.title h1 {
  font-size: 30px;
}

.content {
  padding: 20px 100px;
  min-height: 200px;
}

/* Footer */
.footer {
  height: 50px;
  display: flex;
  justify-content: center;
  background: white;
  font-size: 20px;
}

.link-container {
  flex-wrap: wrap;
  flex-direction: column;
}

.link {
  text-decoration: none;
  padding: 10px;
  margin: 20px;
  background-color:#f48051;
  color: white;
  border-radius: 5px;
}

.footer a {
  color: white;
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
  font-size: 23px;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>
