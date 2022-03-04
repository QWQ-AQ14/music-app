<template>
  <div id="app">
   <header>
     <h1>我的音乐</h1>
   </header>
   <main>
     <section class="player">
       <h2 class="song-title">
         test
       </h2>
     </section>
   </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body{
  font-family: sans-serif;
}
header{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  color: #fff;
  background-color: #212121;
}
</style>
