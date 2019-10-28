<template>
<div></div>
</template>

<script>
import { osName } from 'mobile-device-detect'
import { uuid } from 'vue-uuid'

import Amplify, { Auth, Analytics, AWSKinesisProvider } from 'aws-amplify';
import awsmobile from '../aws-exports';

import { EventBus } from '../utils/event-bus.js';

Amplify.configure(awsmobile);

Analytics.addPluggable(new AWSKinesisProvider());

Analytics.configure({
    AWSKinesis: {
        // OPTIONAL -  Amazon Kinesis service region
        region: 'eu-central-1',      
        // OPTIONAL - The buffer size for events in number of items.
        bufferSize: 1000,     
        // OPTIONAL - The number of events to be deleted from the buffer when flushed.
        flushSize: 100,     
        // OPTIONAL - The interval in milliseconds to perform a buffer check and flush if necessary.
        flushInterval: 5000, // 5s      
        // OPTIONAL - The limit for failed recording retries.
        resendLimit: 5
    } 
});

export default {
  components: {
  },
  data () {
    return {
      userID: 'unknown',
      platform: 'unknown',
      // get kinesis stream name from env
      ingestStream: process.env.VUE_APP_KINESIS_INGEST_STREAM
    }
  },
  methods: {
    sendEvent(payload) {
      // eslint-disable-next-line
      console.log("Submitting event...",payload);
      // populate kinesis event data
      let record = {
        event_type: "level-up",
        event_id: uuid.v1(),
        platform: this.platform,
        user_id: this.userID,
        data: {
          level_number: payload.levelNum
        }
      };
      // submit event to kinesis stream 
      Analytics.record({
        data: record,
        partitionKey: this.userID, 
        streamName: this.ingestStream
      }, 'AWSKinesis')
    }
  },
  mounted () {
      EventBus.$on('level-up', (payload) => { this.sendEvent(payload) })
      // get current credentials from Cognito
      Auth.currentCredentials()
        .then((response) => {
          this.userID = response.data.IdentityId
          // eslint-disable-next-line
          console.log("creds", response)
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.log("error getting credentials", error)
        })
      this.platform = osName
      // eslint-disable-next-line
      console.log("os", this.platform)
  }
}
</script>

<style>
</style>