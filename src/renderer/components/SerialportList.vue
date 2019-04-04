<template>
    <div class="serialport-list">
        <h3>串口列表</h3>
        <ul>
            <li
                class="item"
                v-for="(item, index) in list"
                :key="index"
                :class="{active: item.isActive}"
                @click="chooleSerialport(item, index)"
            >
                <span style="color:#ff9900">[未开启]</span>{{item.comName}}
            </li>
        </ul>
    </div>
</template>

<script>
import Serialport from 'serialport'
export default {
    name: 'serialport-list',
    props: {
        serialportList: {
            type: Array,
            default: []
        }
    },
    computed: {
        list() {
            return this.serialportList
        }
    },
    created() {
        this.serialportList[0].isActive = true
    },
    methods: {
        chooleSerialport(item, index) {
            this.list.forEach(item => {
                item.isActive = false
            })
            this.list[index].isActive = true
            this.$set(this.list, index, this.list[index])
            var port = new Serialport(item.comName);
            port.on('open', function () {
                port.write('main screen turn on ', function (err) {
                    if (err) {
                        return console.log('Error on write: ', err.message);
                    }
                    console.log('message written');
                });
            });

            //打开错误将会发出一个错误事件
            port.on('error', function (err) {
                console.log('Error: ', err.message);
            });
        }
    }
}
</script>

<style lang="less" scoped>
.serialport-list {
    padding: 10px;
    .item {
        line-height: 40px;
        padding-left: 10px;
        cursor: pointer;
    }
    .active {
        background-color: aliceblue;
    }
}
</style>
