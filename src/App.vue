<template>
    <div>
        <month-Select :changeSegment="changeMonth"></month-Select>
    </div>
</template>

<script>
import monthSelect from './components/month'

export default {
    methods:{
        //回调选择的月份段
        changeMonth(val){
            console.log(val) //["2016-03-01","2016-03-31"]
        }
    },
    components: {
        monthSelect
    }
}
</script>

<style>
    *{margin:0; padding: 0}
    body{
        font: 14px Arial, Helvetica, sans-serif
    }
</style>
