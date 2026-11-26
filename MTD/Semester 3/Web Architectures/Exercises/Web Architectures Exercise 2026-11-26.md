#web_architectures 

node

serber

```js
"use strict";

const io = require('socket.io')(3000, {
    cors:
    {
        origin: '*'
    }
});

let clientSockets = [];

io.on('connect', (socket) =>
{
   clientSockets.push(socket);

   socket.on('message', messageHandler)
});

function messageHandler(data)
{
    for (let socket of clientSockets)
    {
        socket.send(data);
    }

    console.log(data);
}

console.log("evil stuff is happening");
```

client

```js
<script setup>

import { onMounted, ref } from 'vue';
import { io } from 'socket.io-client';

const text = ref('')
const messages = ref([]);

let socket = null;

onMounted(() =>
{
  socket = io.connect("http://localhost:3333");
  socket.on('message', (message) =>
  {
    messages.value.push(message);
  });
});

function sendMessage()
{
  socket.send(text.value);
  text.value = "";
}

</script>

<template>
  <h1>
    chatttydtredsghhrhrehephökrsdfjhfdsjhfödhjföfdhjprpijhkrölhkjflhjfhjföhjföhjflköjhf
  </h1>

  <input v-model='text'> <br>
  <button @click="sendMessage">Snend</button>

  <p v-if="messages.length === 0"> No messages :/ </p>
  <ul v-else>
    <li v-for="message in messages" :key="message.id">
      {{ message }}
    </li>
  </ul>

</template>

```