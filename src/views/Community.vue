<template>
  <div class="Community__wrapper">
    <community_channel_Selector class="selector"  :selectedChannel="selectedChannel" />
    <community_channel :channelMessages="messages"/>
  </div>
</template>

<script setup name="Community">
import community_channel from '@/components/community/community_channel.vue';
import community_channel_Selector from '@/components/community/community_channel_Selector.vue';
import axios from 'axios';
import { ref, provide, watch } from 'vue';

const messages = ref();
const selectedChannel = ref(1);

const axiosInstance = axios.create({
  baseURL: 'http://localhost:8000/api',
  withCredentials: true,
  headers: {
    "accept": 'application/json',
    "content-type": "application/json"
  }
})

// call the api to get the messages from the selected channel

const getMessages = (channel_id) => {
  axiosInstance.get(`/channel/${channel_id}`)
  .then((response) => {
    messages.value = response.data.messages
    console.log(messages.value)
  })
  .catch((error) => {
    console.log(error)
  })
}
watch(selectedChannel, (newValue) => {
  getMessages(newValue)
})

const updateSelectedChannel = (value)=>{
  selectedChannel.value = value
}

getMessages(selectedChannel.value)
provide('selectedChannel', selectedChannel)
provide('updateSelectedChannel', updateSelectedChannel)

</script>

<style scoped>
.Community__wrapper {
  display: grid;
  grid-template-columns: 0.1fr 1.2fr;
  grid-template-areas: ". .";
  height: 100%;
  margin: 8px;
  border: #2C9B22 2px solid;
  border-radius: 8px;
  background-color: #F4F6F3;
  background-image: url('/src/assets/background_community.svg');
  background-size: cover;
  background-position: center;
}

.selector{
  border-right: #2C9B22 2px solid;
}
</style>
