<template>
<div id="app">
  <div class="header">
    <router-link to="/">
      <div class="logo">
        <img src="/Pic/logo.png" width="50px" height="50px">
      </div>
    </router-link>
    <div class="title">
      <h1>Instagram (Educational Purpose)</h1>
    </div>
    <div class="log">
      <router-link to="/user">Log In</router-link>
    </div>
  </div>
  <div class="content">
    <router-view />
  </div>
  <footer>
      <div class='footer_1'>
        <div class='foot-container'>
          <div class='foot-link'>
            <a href='https://github.com/Tacoma44/Creative4.git' target="_blank">Github Link</a>
          </div>
        </div>
      </div>
    </footer>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  display: flex;
  background-image: url('/Pic/instagram-image.jpg');
  padding: 10px 100px 0px 100px;
  color: black;
  justify-content: center;
}

.log {
  position: absolute;
  left: 90%;
  margin-top: 24px;
}

.title {
  margin-top: 5px;
}

.title h1 {
  font-size: 30px;
}

.logo {
  margin: 10px;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  height: 50px;
  padding: 20px 100px 0px 100px;
  background: #e3e3e3;
  font-size: 12px;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}

footer {
  display: flex;
  width: 50%;
  height: 50px;
  background-color: black;
  justify-content: center;
  bottom: 0;
  position: fixed;
  left: 350px;
}

.foot-link {
  margin-top: 15px;
}
</style>
