<template>
  <div class="container">
    <div id="title">
        <img src="../assets/pomudachi.webp" alt=""><h1>🍂 PomuDoro 🍂</h1> <img src="../assets/pomudachi.webp" alt="">
    </div>
    <div class="timer">
        <svg
        width="163"
        height="165"
        viewBox="-8 -8 184 188"
        fill="none"
        id="first-segment"
      >
        <path
          stroke="#E9D2B1"
          stroke-width="15"
          stroke-linecap="round"
          d="M165.16,163.38A172,172,0,0,0,0,0"
        />
        <path
          id="top-right"
          stroke="#F85959"
          stroke-width="15"
          stroke-linecap="round"
          d="M165.16,163.38A172,172,0,0,0,0,0"
        />
      </svg>
      <svg
        width="163"
        height="165"
        viewBox="-8 -8 184 188"
        fill="none"
        id="second-segment"
      >
        <path
          stroke="#E9D2B1"
          stroke-width="15"
          stroke-linecap="round"
          d="M0,163.34A172,172,0,0,0,164.44,0"
        />
        <path
          id="bottom-right"
          stroke="#F85959"
          stroke-width="15"
          stroke-linecap="round"
          d="M0,163.34A172,172,0,0,0,164.44,0"
        />
      </svg>
      <svg
        width="163"
        height="165"
        viewBox="-8 -8 184 188"
        fill="none"
        id="third-segment"
      >
        <path
          stroke="#E9D2B1"
          stroke-width="15"
          stroke-linecap="round"
          d="M0,0A172,172,0,0,0,165.16,162.61"
        />
        <path
          id="bottom-left"
          stroke="#F85959"
          stroke-width="15"
          stroke-linecap="round"
          d="M0,0A172,172,0,0,0,165.16,162.61"
        />
      </svg>
      <svg
        width="163"
        height="165"
        viewBox="-8 -8 184 188"
        fill="none"
        id="fourth-segment"
      >
        <path
          stroke="#E9D2B1"
          stroke-width="15"
          stroke-linecap="round"
          d="M160.17,0A172,172,0,0,0,0,161.51"
        />
        <path
          id="top-left"
          stroke="#F85959"
          stroke-width="15"
          stroke-linecap="round"
          d="M160.17,0A172,172,0,0,0,0,161.51"
        />
      </svg>
    </div>
    <button>Start</button>
  </div>

</template>

<script>
export default {
    name: 'homeComponent'
}
</script>

<style>
.container {
    height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
}
#title{
    margin-top: 50px;
    display: flex;
    flex-direction: row;
    align-items: center;
}
h1{
    font-size: 64px;
    color: #c44747;
    margin-right: 10px;
    margin-left: 10px;
}
.timer{
    position: relative;
    margin-top: 100px;
    width: 330px;
    height: 330px;
}
button{
    margin-top: 50px;
    width: 200px;
    height: 68px;
    background: #f85959;
    border-radius: 20px;
    border: none;
    cursor: pointer;

    font-size: 36px;
    line-height: 45px;
    text-align: center;
    color: #fff8ee;

}
button:focus{
    outline: none;
}
</style>