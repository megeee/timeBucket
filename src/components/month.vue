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
                return new Date(year,month,0).getDate()
            };
            //日期格式化
            let formatDate = function(date){
                let year = date.getFullYear()+'-',
                    month = (date.getMonth()+1) < 10 ? '0' + (date.getMonth()+1) + '-' : (date.getMonth()+1) + '-',
                    day = date.getDate() < 10 ? '0' + date.getDate() : date.getDate();
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
                let now = new Date(),
                    lastMonth = new Date( now.getFullYear(), now.getMonth()+n, now.getDate() );
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

</style>
