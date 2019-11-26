<template>
  <div class="test">
    <div class="laber">npm install @antv/my-f2</div>
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
      height: 500,
      // canvas: ""
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
        // 获取画布实际宽高
        // 必要！按照设置的分辨率进行放大
        const myCtx = my.createCanvasContext("area");

        const canvas = new F2.Renderer(myCtx);
        // console.log('canvas*******', canvas)
        //console.log(res[0].width, res[0].height);
        this.drawChart(canvas, res[0].width, res[0].height);
        const canvasWidth = res[0].width * pixelRatio;
        const canvasHeight = res[0].height * pixelRatio;
        // 高清解决方案
        // this.setData({
        //   width: canvasWidth * pixelRatio,
        //   height: canvasHeight * pixelRatio
        // });
        // console.log('*******', canvasWidth, canvasHeight, pixelRatio)
        this.width = canvasWidth
        this.height = canvasHeight
        // console.log('*******', this.width, this.height, pixelRatio)
        console.log('bbbb', myCtx);
        console.log('aaaa', myCtx.scale);
        myCtx.scale(pixelRatio, pixelRatio);
      });
  },
  methods: {
    drawChart (canvas, width, height) {
      const data = [
        { value: 63.4, city: "New York", date: "2011-10-01" },
        { value: 62.7, city: "Alaska", date: "2011-10-01" },
        { value: 72.2, city: "Austin", date: "2011-10-01" },
        { value: 58, city: "New York", date: "2011-10-02" },
        { value: 59.9, city: "Alaska", date: "2011-10-02" },
        { value: 67.7, city: "Austin", date: "2011-10-02" },
        { value: 53.3, city: "New York", date: "2011-10-03" },
        { value: 59.1, city: "Alaska", date: "2011-10-03" },
        { value: 69.4, city: "Austin", date: "2011-10-03" }
        // ...
      ];
      var chart = new F2.Chart({
        el: canvas,
        width,
        height
      });

      chart.source(data, {
        date: {
          range: [0, 1],
          type: "timeCat",
          mask: "MM-DD"
        },
        value: {
          max: 300,
          tickCount: 4
        }
      });
      chart.axis("date", {
        label (text, index, total) {
          const textCfg = {};
          if (index === 0) {
            textCfg.textAlign = "left";
          }
          if (index === total - 1) {
            textCfg.textAlign = "right";
          }
          return textCfg;
        }
      });
      chart
        .area()
        .position("date*value")
        .color("city")
        .adjust("stack");
      chart
        .line()
        .position("date*value")
        .color("city")
        .adjust("stack");
      chart.render();
      return chart;
    },

    touchStart (e) {
      if (this.canvas) {
        this.canvas.emitEvent("touchstart", [e]);
      }
    },
    touchMove (e) {
      if (this.canvas) {
        this.canvas.emitEvent("touchmove", [e]);
      }
    },
    touchEnd (e) {
      if (this.canvas) {
        this.canvas.emitEvent("touchend", [e]);
      }
    }
  }
};
</script>

<style scoped>
.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.userinfo-nickname {
  color: #aaa;
}

.usermotto {
  margin-top: 150px;
}

.form-control {
  display: block;
  padding: 0 12px;
  margin-bottom: 5px;
  border: 1px solid #ccc;
}
.all {
  width: 7.5rem;
  height: 1rem;
  background-color: blue;
}
.all:after {
  display: block;
  content: "";
  clear: both;
}
.left {
  float: left;
  width: 3rem;
  height: 1rem;
  background-color: red;
}

.right {
  float: left;
  width: 4.5rem;
  height: 1rem;
  background-color: green;
}
canvas {
  width: 100%;
  height: 100%;
}
</style>
