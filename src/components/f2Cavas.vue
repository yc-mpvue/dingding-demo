<template>
  <div class="f2Cavas">
    <canvas id="area"
      @TouchStart="touchStart"
      @TouchMove="touchMove"
      @TouchEnd="touchEnd"
      :width="width"
      :height="height" />
  </div>
</template>
<script>
import F2 from "@antv/my-f2";

export default {
  data () {
    return {
      width: 750,
      height: 500
    };
  },
  onReady () {
    my
      .createSelectorQuery()
      .select("#area")
      .boundingClientRect()
      .exec(res => {
        // 获取分辨率
        const pixelRatio = my.getSystemInfoSync().pixelRatio;
        const myCtx = my.createCanvasContext("area");
        const canvas = new F2.Renderer(myCtx);

        this.$emit('drawChart', canvas, res[0].width, res[0].height);

        //按照分辨率(实际宽(750rpx)高的倍数)设置画布宽高,然后再对画布进行缩小(倍数为分辨率值),保证在高分辨率下图表不会模糊
        // 设置画布宽高
        this.width = res[0].width * pixelRatio;
        this.height = res[0].height * pixelRatio;
        myCtx.scale(pixelRatio, pixelRatio);//缩放画布
      });
  },

}
</script>
<style  scoped>
canvas {
  width: 750rpx;
  height: 500rpx;
}
</style>