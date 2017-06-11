<template>
  <div class="login-page-body" v-if="notLoggedIn">
    <login @submit="loginSubmit" :error="error"></login>
  </div>
  <div class="login-done-body" v-else>
    <straming-status 
      @reconnect="reconnect" 
      @logout="logout" 
      :status="status" 
      :disconnected="disconnected"></straming-status>
  </div>
</template>

<script>
  import Login from './LandingPageView/Login'
  import StramingStatus from './LandingPageView/StramingStatus'

  const robot = require('robotjs');
  const shell = require('shelljs');
  const electron = require('electron');
  const globalUrl = '127.0.0.1';

  const w = electron.screen.getPrimaryDisplay().size.width;
  const h = electron.screen.getPrimaryDisplay().size.height;

  let client = {};

  let streamingProcess = {};

  export default {
    components: {
      Login,
      StramingStatus
    },
    name: 'landing-page',
    data: function() {
      return {
        notLoggedIn: true,
        user: null,
        error: '',
        status: '',
        username: '',
        password: '',
        disconnected: false
      }
    },
    methods: {
      reconnect() {
        this.logout();
        this.loginSubmit(this.username, this.password);
      },
      logout() {
        client.onerror = function() {};
        client.onclose = function() {};
        if (typeof client.close === 'function') {
          client.close();
        }
        this.notLoggedIn = true;
        this.user = null;
        this.error = '';
        this.status = '';
        if (typeof streamingProcess.kill === 'function') {
          streamingProcess.kill('SIGINT');
        }
      },
      disconnect() {
        this.disconnected = true;
        this.status = 'Disconnected';
        if (typeof streamingProcess.kill === 'function') {
          streamingProcess.kill('SIGINT');
        }
        client.onerror = function() {};
        client.onclose = function() {};
        if (typeof client.close === 'function') {
          client.close();
        }
      },
      connectToSocket() {
        client = new WebSocket('ws://' + globalUrl + ':8085/' + encodeURI(this.username) + '/' + encodeURI(this.password));

        client.onopen = () => {
          this.status = 'Connected';
        };

        client.onmessage = (event) => {
          var obj = JSON.parse(event.data);

          var type = obj.type;

          if (type === 'move') {
            var x = obj.x;
            var y = obj.y;
            robot.moveMouse(x * w, y * h);
          }
          if (type === 'click') {
            robot.mouseClick();
          }
          if (type === 'text') {
            robot.typeString(obj.value);
          }
          if (type === 'enter') {
            robot.keyTap('enter');
          }
        };
        client.onerror = () => {
          this.disconnect();
        };
        client.onclose = () => {
          this.disconnect();
        };
      },
      streamDesktop() {
        streamingProcess = shell.exec('ffmpeg -thread_queue_size 50 -f avfoundation -framerate 30 -i "1" -video_size 2160x1350 -s 2160x1350 -f mpeg1video -b:v 800k -r 30 http://' + globalUrl + ':8082/' + encodeURI(this.username) + '/' + encodeURI(this.password) + '/2160/1350/', (code, stdout, stderr) => {
          if (stderr && code !== 255) {
            this.disconnect();
          }
        });
      },
      loginSubmit(username, password) {
        this.notLoggedIn = true;
        this.client = null;
        this.status = 'Connecting';
        this.error = '';
        this.username = username;
        this.password = password;

        const options = {
          params: {
            username: username,
            password: password
          }
        };

        this.$http.get('http://' + globalUrl + ':8087/rest/login-client', options).then((response) => {
          this.status = 'Connecting';
          this.notLoggedIn = false;

          this.user = JSON.parse(response.bodyText).client;

          this.disconnected = false;

          setTimeout(() => {
            this.streamDesktop();
          }, 0);

          this.connectToSocket();
        }).catch(() => {
          this.error = 'Unesite ispravne podatke';
        });
      }
    }
  }
</script>

<style scoped>
  img {
    margin-top: -25px;
    width: 450px;
  }
</style>
