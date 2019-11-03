<template>
    <div class="packing noselect">
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
            <van-icon name="clock-o" color="#878787" size="25px" @click="calcHistory" />
            <van-icon
                :name="formulaList.length == 0 ? back_0 : back_1"
                color="#878787"
                size="34px"
                @click="backspace"
            />
        </div>

        <!-- 输入区 -->
        <van-row gutter="10" class="buttonBox">
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
            result: '',
            // 退格图片
            back_0: require('@/assets/backspace_0.png'),
            back_1: require('@/assets/backspace_1.png')
        }
    },

    methods: {
        // 输入计算式
        inputs(info) {
            switch (info.type) {
                case 'reset':
                    this.formulaList = []
                    this.formula = ''
                    this.result = ''
                    break
                case 'equal':
                    this.outputs()
                    break
                case 'symbol':
                    this.formulaList.push(info)
                    this.formula += info.value
                    break
                case 'number':
                    this.formulaList.push(info)
                    this.formula += info.value
                    this.realTimeCalc()
                    break
            }
        },
        // 实时计算
        realTimeCalc(flag) {
            try {
                this.result = this.formula != '' ? String(eval(this.formula)) : ''
                return true
            } catch (error) {
                this.result = ''
                if (flag) {
                    this.$notify({ type: 'danger', message: '恰西？？？' })
                }
                return false
            }
        },
        // 按下等于号输出结果
        outputs() {
            let isSuccess = this.realTimeCalc(true)
            if (!isSuccess) {
                return
            }
            this.formulaList = []
            for (let i = 0; i < this.result.length; i++) {
                let item = {
                    label: this.result[i],
                    value: this.result[i],
                    type: 'number'
                }
                this.$set(this.formulaList, i, item)
            }
            this.formula = this.result
            this.result = ''
        },
        // 退格
        backspace() {
            if (this.formulaList.length == 0) {
                return
            }
            this.formulaList.pop()
            this.formula = this.formula.substr(0, this.formula.length - 1)
            this.realTimeCalc()
        },
        // 计算历史
        calcHistory() {
            this.$notify({ type: 'primary', message: '在做，莫急' })
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
    margin-bottom: 20px;
    flex: none;
    height: 10%;
    overflow-x: auto;
    display: flex;
    justify-content: flex-end;
    align-items: center;
}

.content_top {
    font-size: 30px;
}

.content_bottom {
    color: #878787;
    font-size: 20px;
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
        height: 100%;
        line-height: calc((100vh - 40px) / 10);
        text-align: center;
        border-radius: 50%;

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
    // font-weight: bold;
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
