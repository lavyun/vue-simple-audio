A simple music player component base on Vuejs
----------


### Effect:

![](http://p1.bqimg.com/567571/568fff0ed900b41a.png)

[demo](http://lavyun.github.io/vue-simple-audio/)


### How to use

1. install with npm

```
npm install --save vue-simple-audio
```

2. use it in your component

```html
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <hello></hello>
    <vue-simple-audio 
        :songs="songs" 
        width="500" 
        initial-volume="40" 
        loop auto-play
        repeat
        bg-color="#345345">
    </vue-simple-audio>
  </div>
  </div>
</template>

<script>
import VueSimpleAudio from 'vue-simple-audio'

export default {
  name: 'app',
  components: {
    'vue-simple-audio':VueSimpleAudio
  },
  data(){
    return {
      songs: [{
        "url": "http://fs.open.kugou.com/7840167216f9b80284d2bb2a9bd9554b/58ac0322/G076/M0A/0C/1D/7IYBAFgu5wmAOS2gAEuViOk9tuk748.mp3",
        "songname": "林俊杰-你的唯一"
      }, {
        "url": "http://fs.open.kugou.com/aecae9884f5c9ca2063864958ddb799e/58ac03ae/G008/M07/06/09/qIYBAFUK1BuAB-aSADmE5bqELvQ099.mp3",
        "songname": "Maroon-sugar"
      }]
    }
  }
}
</script>
```

  
| params        |description               | default  |
| ------------- |:-------------:| -----:|
| songs      | A array of objects,it contains song's infomation,it has two property,the song's url and name | -- |
| auto-play      | auto play?      |  true |
| repeat | repeat the list?      |  true |
| loop | repeat one?      |  false |
| width | the player's width      |  300 |
| initial-volume | volume      |  60 |
| bg-color |the player's color| rgb(0,0,0)|


