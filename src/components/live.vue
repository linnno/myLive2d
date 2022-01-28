<template>
  <div class="myLive">
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script>
import { Application } from '@pixi/app';
import { Renderer } from '@pixi/core';
import { Ticker, TickerPlugin } from '@pixi/ticker';
import { InteractionManager } from '@pixi/interaction';
import { Live2DModel } from 'pixi-live2d-display';
Application.registerPlugin(TickerPlugin);
Live2DModel.registerTicker(Ticker);
// 注册 InteractionManager 以支持 Live2D 模型的自动交互
Renderer.registerPlugin('interaction', InteractionManager);
export default {
  name: 'live2d',
  methods: {
    async init() {
      const app = new Application({
        antialias: true, width: 800, height: 800, backgroundAlpha: 0, resolution: 1, transparent: true,
        view: this.$refs.canvas,
        autoStart: true
      });

      const model = await Live2DModel.from('/model/bisimai_2/bisimai_2.model3.json');
      model.x = 0;
      model.y = 0;
      model.scale.set(0.1, 0.1);
      model.anchor.set(0.1, 0.1);

      model.on('click', () => {
        model.motion('touch_head');
      })
      
      app.stage.addChild(model);
    }
  },
  mounted() {
    this.init()
  }
}
</script>

<style scoped>
  .myLive{
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>
