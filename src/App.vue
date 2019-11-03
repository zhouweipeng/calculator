<template>
    <div id="app">
        <router-view />
    </div>
</template>

<style lang="less" scope>
#app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    color: #454545;
    background: #fcfcfc;
    height: 100vh;
    width: 100vw;
}
</style>

<style lang="less">
// 禁止用户选择文字
.noselect {
    -moz-user-select: none;
    -khtml-user-select: none;
    user-select: none;
}
</style>
