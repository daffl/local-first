<script setup lang="ts">
import { ref } from 'vue'
import { isValidAutomergeUrl, Repo } from '@automerge/automerge-repo'
import { BroadcastChannelNetworkAdapter } from '@automerge/automerge-repo-network-broadcastchannel'
import { BrowserWebSocketClientAdapter } from "@automerge/automerge-repo-network-websocket"
import { IndexedDBStorageAdapter } from "@automerge/automerge-repo-storage-indexeddb"

const repo = new Repo({
  network: [
    new BroadcastChannelNetworkAdapter(),
    new BrowserWebSocketClientAdapter('wss://sync.automerge.org')
  ],
  storage: new IndexedDBStorageAdapter(),
})

const username = ref('')
const text = ref('')

type Message = {
  id: string
  username: string
  text: string
  date: string
}

function addMessage () {
  handle.change(doc => {
    if (username.value && text.value) {
      doc.messages.push({
        id: Math.random().toString(36).substring(7),
        username: username.value,
        text: text.value,
        date: new Date().toLocaleString()
      })
      text.value = ''
    }
  })
}

const messages = ref<Message[]>([])

const hash = `${document.location.hash.substring(1)}`

if (isValidAutomergeUrl(hash)) {
  window.localStorage.setItem('automerge:repo', hash)
  document.location.hash = ''
} 

const rootDocUrl = window.localStorage.getItem('automerge:repo')

let handle

if (isValidAutomergeUrl(rootDocUrl)) {
  console.log('Loading automerge repo', rootDocUrl)
  handle = repo.find(rootDocUrl)
} else {
  handle = repo.create()
  handle.change(doc => {
    doc.messages = []
  })
  console.log('Created automerge repo', handle.url)
  window.localStorage.setItem('automerge:repo', handle.url)
}

handle.on('change', ({doc}) => {
  messages.value = doc.messages
  document.getElementById('messages')!.scrollTo(0, document.getElementById('messages')!.scrollHeight)
})
</script>

<template>
<div class="container mx-auto py-4 flex flex-col h-full">
  <form @submit.prevent="addMessage" class="pb-4 flex items-center">
    <input type="text" v-model="username" placeholder="GitHub username" class="border p-2 mr-2 rounded">
    <!-- Message Input Field with Flex Grow -->
    <input type="text" v-model="text" placeholder="Message" class="border p-2 flex-grow rounded mr-2">
    <button type="submit" class="bg-orange-400 text-white p-2 rounded">Send</button>
  </form>
  <div class="flex-grow overflow-auto border border-gray-500 rounded-sm" id="messages">
    <div v-for="message in messages" :key="message.id" class="flex items-center p-2">
      <img :src="'https://github.com/' + message.username + '.png'" class="w-15 h-15 m-4 rounded-full object-cover" alt="Avatar Image" />

      <div class="flex-grow">
        <!-- Username and Date -->
        <div class="flex justify-between">
          <span class="font-bold">{{ message.username }}</span>
          <span class="text-sm text-gray-600">{{ message.date }}</span>
        </div>
        <!-- Message Text -->
        <div class="bg-gray-400 text-black p-3 rounded-lg mt-1 max-w-xs">
          {{ message.text }}
        </div>
      </div>
    </div>
  </div>
</div>
</template>
