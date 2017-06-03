<template>
<div>
  <text-box
    v-for="message in messages"
    :message="message"
    :key="message.id"
  ></text-box>
  <input placeholder="Enter chat here" v-model="chat">
  <button v-on:click="sendMessage"> Send </button>
</div>
</template>


<script>
import TextBox from './TextBox.vue'
import io from '../../../../node_modules/socket.io-client/dist/socket.io.js';

export default {

  props: ['socket'],

  watch: {
    socket: function() {
      this.startSocketIO();
    }

  },

  // State variables.
  // ================
  data: function () {
    return {
      messages: [],
      chat: '',
      verbose: true // On/off flag for log() method
    }
  },

  // Controller methods.
  // ===================
  methods: {

    // Convenience method for logging debugging messages
    log: function() {
      this.verbose && console.log.apply(console, arguments);
    },

    // Connect to Signal Server and initiate listeners
    startSocketIO: function() {
      this.socket.on('message', (message) => {
        console.log('ENORMOUS SUCCESSS!', this.messages, message);
        this.messages.push(message);
      });

    },

    sendMessage: function() {

      if (this.socket !== null) {

        console.log('sendMessage: ', this.socket.id);

        var data = {
          room: 'test',
          message: {
            id: this.messages.length,
            text: this.chat
          }
        };

        this.socket.emit('message', data);
        this.messages.push(data.message);
      } else {
        console.log('WARNING! FAIL!!');
      }

      this.chat = '';
    }
  },

  components: {
    TextBox
  }

}

</script>


<style>

</style>