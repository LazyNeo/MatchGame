<template>
  <div @click="turned"  class="boxF  ">
    <div class="boxS">
      <div class="boxT">
        <div style="position:relative;overflow:hidden;height:100%;">
          <li v-show="showList[0]" class="line line-1"></li>
          <li v-show="showList[1]" class="line line-2"></li>
          <li v-show="showList[2]" class="line line-3"></li>
          <li v-show="showList[3]" class="line line-4"></li>
          <li v-show="showList[4]" class="line line-5"></li>
          <li v-show="showList[5]" class="line line-6"></li>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      // note: changing this line won't causes changes
      // with hot-reload because the reloaded component
      // preserves its current state and we are modifying
      // its initial state.
      msg: 'Hello World! child',
      // showList: [false, true, true, true, true, true],
      show: true
    }
  },
  props: {
    showList: {
      twoWay: true,
      type: Array,
      default: function () {
        return [false, true, false, true, false, true]
      }
    },
    pMsg: {
      type: String
    },
    active: {
      twoWay: true,
      type: Boolean,
      default: false
    },
    initItem: {
      type: Boolean,
      default: false
    },
    x: {
      type: Number
    },
    y: {
      type: Number,
      default: 0
    }
  },
  methods: {
    turned: function () {
      let showList = this.showList
      let newList = []
      for (let i = 0; i < showList.length; i++) {
        newList[(i + 1) % 6] = showList[i]
      }
      this.showList = newList
      this.$emit('item-click', this.x, this.y)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
    @DEG: 0deg;
    .boxF, .boxS, .boxT{
        width: 100px;
        height: 125px;
        overflow: hidden;
        visibility: hidden;
    }
    .boxF{
        background-color: yellow;
        transform: rotate(@DEG 90deg);
        float: left;
        .boxS{
            background-color: red;
            transform: rotate(@DEG - 60deg);
            .boxT{
              background-color: #334455;
              transform: rotate(@DEG - 60deg);
              visibility: visible;
            }
        }
    }
    .line{
      height: 16px;
      width: 125px;
      top:57px;
      left: -12.5px;
      line-height: 16px;
      position: absolute;;
      display: block;
      visibility: visible;
      &:before,&:after{
        content: "";
        background-color: transparent;;
        display: block;
        float: left;
        height: 100%;
        width: 50%;
      }
      &:after{
        background-color: green;
      }
    }
    .non-active .line:after{
      background-color: yellow;
    }

    .line-1{
      transform: rotate(@DEG);
    }
    .line-2{
      transform: rotate(@DEG + 60deg);
    }
    .line-3{
      transform: rotate(@DEG + 120deg);
    }
    .line-4{
      transform: rotate(@DEG + 180deg);
    }
    .line-5{
      transform: rotate(@DEG + 240deg);
    }
    .line-6{
      transform: rotate(@DEG + 300deg);
    }
</style>
