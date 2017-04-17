<template>
    <div>
        <month-Select :changeSegment="changeDate"></month-Select>
        <week-Select :changeSegment="changeDate"></week-Select>
    </div>
</template>

<script>
import monthSelect from './components/month'
import weekSelect from './components/week'
export default {
    methods:{
        //回调选择的月份段
        changeDate(val){
            console.log(val) //["2016-03-01","2016-03-31"]
        }
    },
    components: {
        monthSelect,weekSelect
    }
}
</script>

<style>
    *{margin:0; padding: 0}
    body{
        font: 14px Arial, Helvetica, sans-serif
    }
    .time_bucket{
        height: 45px;
        display: flex;
        justify-content:space-between;
        align-items:center;
        background: #1D8CE0;
        color: #fff;
        margin-bottom: 3px
    }
    .time_bucket > div{
        line-height: 45px;
        padding: 0 10px
    }
    .time_bucket > div:active{
        background: #2c94e2
    }
    .time_bucket .none{
        pointer-events: none;
        color: #ccc
    }
</style>
