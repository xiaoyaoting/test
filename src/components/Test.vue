<template>
  <div class="test-container">
    <van-field v-model="hash" label="Block Hash" placeholder="Search Blockchain, Transactions, Addresses and Blocks">
      <template #button>
        <van-button size="small" type="primary" @click="getBlockAndTxList">search</van-button>
      </template>
    </van-field>
    <div class="container">
        <div class="container-msg">
          <van-list v-model="blockData" :finished="true" finished-text="没有更多了">
            <van-cell v-for="(val, key) in blockData" :key="key">
              <div class="list-item">
                <div class="list-item-label">{{ key }}</div>
                <div class="list-item-dot">:</div>
                <div class="list-item-text">{{ val }}</div>
              </div>
            </van-cell>
          </van-list>
        </div>
        <div class="container-list">
          <van-collapse v-model="activeNames">
            <van-collapse-item v-for="item in txData" :key="item.hash" :name="item.hash">
              <template #title>
                <div class="tx-item">
                  <div>block_height: {{ item.block_height }}</div>
                  <div>block_index: {{ item.block_index }}</div>
                  <div>time: {{ item.time }}</div>
                </div>
              </template>
              hash: {{ item.hash }}
            </van-collapse-item>
          </van-collapse>
        </div>
    </div>
  </div>
</template>

<script>
import axios from '../libs/api'
import dayjs from 'dayjs'
export default {
  name: 'Test',
  props: {
    msg: String,
  },
  data() {
    return {
      hash: '',
      blockData: {},
      loading: true,
      txData: [],
      activeNames: []
    }
  },
  mounted() {
    this.getBlockAndTxList()
  },
  methods: {
    async getBlockAndTxList() {
      const data = await axios(`https://blockchain.info/rawblock/${this.hash}`)
      console.log(data)
      this.blockData = Object.keys(data.data)
        .filter((key) => !key.includes("tx"))
        .reduce((obj, key) => {
          return Object.assign(obj, {
            [key]: data.data[key]
          });
        }, {});
      this.txData = data.data.tx.map(item => ({ ...item, time: dayjs.unix(item.time).format('YYYY:MM:DD HH:mm:ss') }))
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.test-container {
  width: 100%;
  height: 100%;
}

.container {
  width: 100%;
  height: 100%;
  display: flex;
}

.container-msg {
  width: 50%;
  height: 100%;
}

.container-list {
  width: 50%;
  height: 100%;
}

.list-item {
  display: flex;
}

.list-item-label {
  font-weight: 800;
}

.list-item-dot {
  padding: 0 10px;
}

.tx-item {
  display: flex;
}

.tx-item>div {
  width: 30%;
}
</style>
