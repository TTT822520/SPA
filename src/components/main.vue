<template>
  <div>
    <transition :name="transitionName">
      <keep-alive>
        <router-view class="child-view"></router-view>
      </keep-alive>

    </transition>

    <radius></radius>  


    <mu-paper>
      <mu-bottom-nav :value="activeTab" @change="onChange" class="tabs">
        <template v-for="o,index of items">
             <mu-bottom-nav-item :value="o.name" :title="o.title" :icon="o.icon"/>
        </template>
      </mu-bottom-nav>
    </mu-paper>


  </div>
</template>

<script>
  import Radius from '@/components/common/radius.vue'

  
  export default {
    data() {
      const activeNames = ['home','doutu','doutu1','person'],
          active = 0,
          activeTab = activeNames[active]
      return {
        transitionName : 'slide-left',
        active,
        activeNames,
        activeTab,
        items:[{
          title: '首页',
          name: 'home',
          icon: 'home'
        },{
          title: '斗图',
          name: 'doutu',
          icon: 'image'
        },{
          title: '列表',
          name: 'doutu1',
          icon: 'list'
        },{
          title: '我的',
          name: 'person',
          icon: 'person'
        }]
      }
    },
    methods: {
      onChange(name) {
        let wantIndex = this.activeNames.indexOf(name)
        if(wantIndex < this.active){
          this.$router.goto(name,true)
        }else{
          this.$router.goto(name)
        }

        this.activeTab = name
        this.active = wantIndex
      }
    },
    watch: {
      '$route' (to, from, next) {
        this.transitionName = this.$router.isBack ? 'slide-right' : 'slide-left'
        this.$router.isBack = false
      }
    },
    activated() {
      let name = this.$route.name,
        wantIndex = this.activeNames.indexOf(name)
        
      this.activeTab = name
      this.active = wantIndex
    },
    //添加新闻列表组件
    components: {
      Radius
    }
  }

</script>

<style scoped>
  .tabs {
    position: absolute;
    left: 0;
    bottom: 0;
  }

  .child-view {
    position: absolute;
    height: 100%;
    width: 100%;
    transition: all .2s cubic-bezier(.55, 0, .1, 1);
  }

  .slide-left-enter,
  .slide-right-leave-active {
    opacity: .2;
    -webkit-transform: translate3d(80%, 0, 0);
    transform: translate3d(80%, 0, 0);
    
  }



  .slide-left-leave-active,
  .slide-right-enter {
    opacity: .2;
    -webkit-transform: translate3d(-80%, 0, 0);
    transform: translate3d(-80%, 0, 0);
  }



  

</style>
