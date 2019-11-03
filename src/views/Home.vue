<template>
    <div class="packing">
        <!-- 上部内容框 -->
        <div class="content content_top">
            <div
                v-for="(item, index) in formulaList"
                :key="index"
                :class="{symbol: item.type == 'symbol'}"
            >{{ item.label }}</div>
        </div>

        <!-- 下部内容框 -->
        <div class="content content_bottom">{{ result }}</div>

        <!-- 功能栏 -->
        <div class="toolBar">
            <van-icon name="clock-o" color="#878787" size="25px" />
            <van-icon name="arrow-left" color="#878787" size="25px" @click="backspace" />
        </div>

        <!-- 输入区 -->
        <van-row gutter="10" class="buttonBox noselect">
            <van-col span="6" v-for="(item, index) in buttonList" :key="index">
                <div
                    :class="{btn: true, reset: item.type == 'reset', symbol: item.type == 'symbol', equal: item.type == 'equal'}"
                    @click="inputs(item)"
                >{{ item.label }}</div>
            </van-col>
        </van-row>
    </div>
</template>

<script>
export default {
    name: 'home',

    data() {
        return {
            // 按钮组
            buttonList: [
                { label: 'C', value: 'reset', type: 'reset' },
                { label: '(', value: '(', type: 'symbol' },
                { label: ')', value: ')', type: 'symbol' },
                { label: '÷', value: '/', type: 'symbol' },
                { label: '7', value: '7', type: 'number' },
                { label: '8', value: '8', type: 'number' },
                { label: '9', value: '9', type: 'number' },
                { label: '×', value: '*', type: 'symbol' },
                { label: '4', value: '4', type: 'number' },
                { label: '5', value: '5', type: 'number' },
                { label: '6', value: '6', type: 'number' },
                { label: '-', value: '-', type: 'symbol' },
                { label: '1', value: '1', type: 'number' },
                { label: '2', value: '2', type: 'number' },
                { label: '3', value: '3', type: 'number' },
                { label: '+', value: '+', type: 'symbol' },
                { label: '%', value: '%', type: 'number' },
                { label: '0', value: '0', type: 'number' },
                { label: '.', value: '.', type: 'number' },
                { label: '=', value: 'calc', type: 'equal' }
            ],
            // 用于展示内容的计算式
            formulaList: [],
            // 用于计算结果的计算式
            formula: '',
            // 结果
            result: ''
        }
    },

    methods: {
        // 输入计算式
        inputs(info) {
            switch (info.type) {
                case 'reset':
                    this.formulaList = []
                    this.formula = ''
                    break
                case 'equal':
                    this.outputs()
                    break
                default:
                    this.formulaList.push(info)
                    this.formula += info.value
            }
        },
        // 输出结果
        outputs() {
            try {
                this.result = eval(this.formula)
            } catch (error) {
                this.$notify({ type: 'danger', message: '???' })
            }
        },
        // 退格
        backspace() {
            this.formulaList.pop()
            this.formula = this.formula.substr(0, this.formula.length - 1)
        }
    }
}
</script>

<style lang="less" scope>
.packing {
    width: calc(100% - 40px);
    height: calc(100% - 40px);
    padding: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.content {
    // width: 100%;
    margin-bottom: 20px;
    flex: none;
    height: 10%;
    font-size: 30px;
    overflow-x: auto;
    display: flex;
    justify-content: flex-end;
    align-items: center;
}

.toolBar {
    flex: none;
    height: 10%;
    box-sizing: border-box;
    border-bottom: 1px solid #f3f3f3;
    display: flex;
    justify-content: space-between;
    align-items: center;

    .van-icon {
        width: calc((100% - 30px) / 4);
        text-align: center;
        border-radius: 12px;

        &:active {
            background: #c3c3c3;
        }
    }
}

.buttonBox {
    flex: none;
    height: 60%;
}

.van-col {
    height: 20%;
}

.btn {
    width: 100%;
    height: calc(100% - 10px);
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
    background: #f4f4f4;
    margin-top: 10px;
    font-size: 30px;
    font-weight: bold;
    transition: all 0.1s;

    &:active {
        background: #c3c3c3;
        font-size: 25px;
    }
}

// 清空按钮样式
.reset {
    color: #e4684c;
}

// 运算符样式
.symbol {
    color: #398628;
}

// 等于号样式
.equal {
    background: #58911c;
    color: #fff;

    &:active {
        background: #467316;
        font-size: 25px;
    }
}
</style>
