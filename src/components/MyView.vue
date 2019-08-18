<template>
    <div class="myview">
        <div class="alert alert-danger" role="alert">
            A simple danger alert—check it out!
        </div>
        <h1>{{ msg }}</h1>
        <button v-on:click="greet()">Button</button>
        <ul id="example-1">
            <li v-for="item in list_data">
                {{ item }}
            </li>
        </ul>
    </div>
</template>

<script>
import mymixin from '../mymixin.js'

export default {
  name: 'MyView',
  data: function(){
      return {
          "msg": "Hello MyView",
          "list_data": ["foo", "bar", "baz"]
      }
  },
  methods: {
      greet: function(){
          var message = this.getMessage()
          this.msg = message;
      }
  },
  mixins : [ mymixin ]
}
</script>