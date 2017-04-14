<!-- 月时间段选择组件 -->
<template>
    <section class="time_bucket">
        <div @touchstart="dataSegment().pre()">上个月 </div>
        <div>{{ segmentStr[0] }} ~ {{ segmentStr[1] }}</div>
        <div @touchstart="dataSegment().next()" :class="{'none':compareDate(segmentStr[1],new Date())}">下个月</div>
    </section>
</template>

<script>
export default {
    data () {
        return {
            segmentStr : [],
            nowMonth:0
        }
    },
    created (){
        this.dataSegment().init();
    },
    props: {
        changeSegment: { type: Function }
    },
    methods : {
        dataSegment () {  //获取时间段数据
            let _this = this;
            //获取某月最后一天的日期
            let getLastDay = function(year,month) {
                //取当前的年份
                let new_year = year,
                    //取下一个月的第一天，方便计算（最后一天不固定）
                    new_month = month++;
                if(month>12) {
                    new_month -=12;
                    new_year++;
                }
                //取当年当月中的第一天
                let new_date = new Date(new_year,new_month,1);
                //获取当月最后一天日期
                return (new Date(new_date.getTime()-1000*60*60*24)).getDate();
            };
            //日期格式化
            let formatDate = function(date){
                let year = date.getFullYear()+'-';
                let month = (date.getMonth()+1) < 10 ? '0' + (date.getMonth()+1) + '-' : (date.getMonth()+1) + '-';
                let day = date.getDate() < 10 ? '0' + date.getDate() : date.getDate();
                return year+month+day;
            };
            //输出
            let showMonth = function(newData){
                let Y = newData.getFullYear(),
                    M = newData.getMonth()+1,
                    LastDay = getLastDay(Y,M),
                    dateStr = [Y + "-" + (M < 10 ? "0" + M : M) + '-01',Y + '-' + (M < 10 ? "0" + M : M) + '-'+ LastDay];
                _this.segmentStr = dateStr;
                _this.changeSegment(dateStr);
            };
            //切换月份方法
            let changeMonth = function(n){
                let now = new Date();
                let lastMonth = new Date( now.getFullYear(), now.getMonth()+n, now.getDate() );
                return lastMonth;
            };

            let Export = {};
            //初始化
            Export.init = function(now){
                showMonth(changeMonth(_this.nowMonth));
            };
            //下一个月
            Export.pre = function(){
                _this.nowMonth--;
                showMonth(changeMonth(_this.nowMonth));
            };
            //上一个月
            Export.next = function(){
                _this.nowMonth++;
                showMonth(changeMonth(_this.nowMonth));
            };
            return Export;
        },
        //对比月份
        compareDate (d1,d2) {
            let date1 = new Date(d1),
                date2 = new Date(d2);
            return date1.getMonth() >= date2.getMonth() && date1.getFullYear() >= date2.getFullYear();
        }
    }
}
</script>

<style scoped>
    .time_bucket{
        height: 45px;
        display: flex;
        justify-content:space-between;
        align-items:center;
        background: #1D8CE0;
        color: #fff;
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
