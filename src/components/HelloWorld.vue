<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div class="container mt-5 pt-5">
      <div class="clock">
        <span class="hour"> {{ hour }} </span>
        <span class="minute"> {{ minute }} </span>
        <span class="second"> {{ second }} </span>
        <span class="millisecond"> {{ millisecond }} </span>
      </div>
      <div class="col-12 mt-5 pt-5">
        <button class="btn btnCustom btn-success" @click="counter()">Start</button>
        <button class="btn btnCustom btn-info" @click="pause()">Pause</button>
        <button class="btn btnCustom btn-secondary" @click="resume()">Resume</button>
        <button class="btn btnCustom btn-warning" @click="split()">Split</button>
        <button class="btn btnCustom btn-danger" @click="reset()">Reset</button>
      </div>
      <div class="col-12 mt-5">
        <table
          class="table table-responsive table-bordered table-stripped table-hovered"
        >
          <thead>
            <tr>
              <th>#Index</th>
              <th>#Splitted Time</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="s in splittedTimes" :key="s">
              <td>{{ s.id }}</td>
              <td>
                {{ s.hour }} : {{ s.minute }} : {{ s.second }} :
                {{ s.millisecond }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import { use } from "vue/types/umd";

@Component
export default class HelloWorld extends Vue {
  @Prop() private msg!: string;
  private hour = 0;
  private minute = 0;
  private second = 0;
  private millisecond = 0;
  private counterInterval = 0;
  private pauseStatus = true;
  private splittedTimeIndex = 0;
  private splittedTimes = [] as any;

  private updateWatch() {
    if (this.pauseStatus == false) {
      this.millisecond++;
      if (this.millisecond == 100) {
        this.millisecond = 0;
        this.second++;
      }
      if (this.second == 60) {
        this.second = 0;
        this.minute++;
      }
      if (this.minute == 60) {
        this.minute = 0;
        this.hour++;
      }
    }
  }
  private counter() {
    this.pauseStatus = false;
    clearInterval(this.counterInterval);
    this.counterInterval = setInterval(this.updateWatch, 10);
  }
  private pause() {
    this.pauseStatus = true;
  }
  private resume() {
    this.pauseStatus = false;
  }
  private split() {
    this.splittedTimeIndex++;
    this.splittedTimes.push({
      id: this.splittedTimeIndex,
      hour: this.hour,
      minute: this.minute,
      second: this.second,
      millisecond: this.millisecond,
    });
  }
  private reset() {
    this.second = 0;
    this.millisecond = 0;
    this.hour = 0;
    this.second = 0;
    this.pauseStatus = true;
  }
}
</script>

<style scoped lang="scss">
* {
  font-family: "square sans serif";
}
@font-face {
  font-family: "square sans serif";
  src: url("~@/assets/fonts/square_sans_serif_7.ttf");
}
.clock {
  background: rgb(0, 196, 255);
  width: 10em;
  margin: 40px auto 0 auto;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 6em;
  border-radius: 18px;
  color: white;
  position: relative;
  &:before,
  &:after {
    content: "";
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    width: calc(100% + 30px);
    background: inherit;
    height: calc(100% + 30px);
    border-radius: inherit;
    opacity: 0.4;
    z-index: -1;
  }
  &:after {
    width: calc(100% + 60px);
    height: calc(100% + 60px);
    opacity: 0.2;
  }
  span {
    margin: 0 0.3em;
    position: relative;
    min-width: 1.8em;
    + span:before {
      content: ":";
      position: absolute;
      left: -0.4em;
      top: 50%;
      transform: translateY(-50%);
    }
  }
}

.btnCustom {
  //background: black;
  color: white;
  margin: 0 1em;
  padding: 0.3em 1.2em;
  text-transform: uppercase;
  font-size: 1.4em;
  &:is(:hover, :focus, :active) {
    //background: #00c4ff;
    //color: white;
  }
}
</style>
