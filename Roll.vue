<template>
  <div>
    <ul class="navs">
      <li :class="{active: active===0}" @click="scrollTo(0)">
        Content-A
      </li>
      <li :class="{active: active===1}" @click="scrollTo(1)">
        Content-B
      </li>
      <li :class="{active: active===2}" @click="scrollTo(2)">
        Content-C
      </li>
      <li :class="{active: active===3}" @click="scrollTo(3)">
        Content-D
      </li>
      <li :class="{active: active===4}" @click="scrollTo(4)">
        Content-E
      </li>
    </ul>

    <div class="content">
      <div>
        Content-A
      </div>
      <div>
        Content-B
      </div>
      <div>
        Content-C
      </div>
      <div>
        Content-D
      </div>
      <div>
        Content-E
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {},
  data() {
    return {
      active: 0 
    }
  },
  mounted() {
   
    window.addEventListener('scroll', this.onScroll, false)
  },
  methods: {
    onScroll() {
      const navContents = document.querySelectorAll('.content div')
      const offsetTopArr = []
      navContents.forEach(item => {
        offsetTopArr.push(item.offsetTop)
      })
      const scrollTop = document.documentElement.scrollTop || document.body.scrollTop
      let navIndex = 0
      for (let n = 0; n < offsetTopArr.length; n++) {
        if (scrollTop >= offsetTopArr[n]) {
          navIndex = n
        }
      }
      this.active = navIndex
    },
    
    scrollTo(index) {
      const targetOffsetTop = document.querySelector(`.content div:nth-child(${index + 1})`).offsetTop
      let scrollTop = document.documentElement.scrollTop || document.body.scrollTop
      const STEP = 50
      if (scrollTop > targetOffsetTop) {
        smoothUp()
      } else {
        smoothDown()
      }
      function smoothDown() {
        if (scrollTop < targetOffsetTop) {
          if (targetOffsetTop - scrollTop >= STEP) {
            scrollTop += STEP
          } else {
            scrollTop = targetOffsetTop
          }
          document.body.scrollTop = scrollTop
          document.documentElement.scrollTop = scrollTop
          requestAnimationFrame(smoothDown)
        }
      }
      function smoothUp() {
        if (scrollTop > targetOffsetTop) {
          if (scrollTop - targetOffsetTop >= STEP) {
            scrollTop -= STEP
          } else {
            scrollTop = targetOffsetTop
          }
          document.body.scrollTop = scrollTop
          document.documentElement.scrollTop = scrollTop
          requestAnimationFrame(smoothUp)
        }
      }
    }
  }
}
</script>

<style scoped>
  .content {
    background-color: white;
    margin:5rem 0 0 0;
  }
  .content div {
    height: 600px;
    font-size: 36px;
    padding: 20px;
    background-color: #7ec384;
  }
  .content div:nth-child(2n) {
    background-color: #847ec3;
  }
  
  .navs {
    position: fixed; top: 0;
    border-radious:20px;
    
  }
  .navs li {
    padding: 0 20px;
    font-size: 24px;
    border-radius: 50px;
    float:left;
    list-style-type :none
  }
  .navs .active{
    color: #847ec3;
    background-color: #e2e2e2;
  }
</style>