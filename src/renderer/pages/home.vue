<template>
    <div class="home">
        <div class="aside">
            <serialport-list :serialportList="serialportList"></serialport-list>
        </div>
        <div class="data">

        </div>
    </div>
</template>

<script>
import serialport from 'serialport'
import SerialportList from '../components/SerialportList'

export default {
    name: 'home',
    components: { SerialportList },
    data() {
        return {
            serialportList: []
        }
    },
    created() {
        this.getSerialportData()
    },
    methods: {
        open(link) {
            this.$electron.shell.openExternal(link)
        },
        getSerialportData() {
            serialport.list().then(
                ports => {
                    this.serialportList = ports
                }
            )
        }
    }
}
</script>

<style lang="less" scoped>
.home {
    .aside {
        position: fixed;
        top: 0;
        left: 0;
        bottom: 0;
        width: 400px;
    }
    .data {
        position: fixed;
        top: 0;
        left: 400px;
        bottom: 0;
        right: 0;
    }
}
</style>

