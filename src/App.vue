<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <TheUserInput @response="updateRepos" @update-type-res="updateTypeOfResponse" :typeresp="typeOfResponse"></TheUserInput>
    <Timeline v-show="typeOfResponse" :repoData="repos"></Timeline>
    <!-- {{this.repos}} -->
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import TheUserInput from './components/TheUserInput.vue';
import Timeline from './components/Timeline.vue';

import { Response } from '@/types';

// Token: 15babcf3ad9ee5a70cf915795e85d575cb6495da

@Component({
  components: {
    TheUserInput,
    Timeline,
  },
})
export default class App extends Vue {
  private repos: Array<object> = [];
  private typeOfResponse = false;

  public updateRepos(repos: Array<object>) {
    this.repos = repos;
  }

  public updateTypeOfResponse(typeOfResponse: boolean) {
    this.typeOfResponse = typeOfResponse;
  }
}
</script>

<style lang="scss">

$app-background: #212121;

* {
  margin:0;
  padding:0;
  box-sizing: border-box;
}

#app {
  text-align: center;
  background-color: $app-background;
  min-height: 100vh;
}
</style>
