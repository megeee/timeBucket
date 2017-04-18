<template>
    <div class="time_bucket">
        <div @touchstart="dataSegment().pre()">上周</div>
        <div>{{ segmentStr[0] }} ~ {{ segmentStr[1] }}</div>
        <div :class="{'none':compareDate(segmentStr[1],maxDay)}" @touchstart="dataSegment().next()">下周</div>
    </div>
</template>
<script>
export default {
    data () {
        return {
            segmentStr : "",
            currentFirstDate: ""
        }
    },
    created (){
        this.dataSegment().init(new Date()); //初始化
    },
    props: {
        changeSegment: { type: Function },
        maxDay:"" //最大可选日期
    },
    methods : {
        dataSegment () {  //获取时间段数据
            let _this = this;
            //格式化时间
            let formatDate = function(date){
                let year = date.getFullYear()+'-';
                let month = (date.getMonth()+1) < 10 ? '0' + (date.getMonth()+1) + '-' : (date.getMonth()+1) + '-';
                let day = date.getDate() < 10 ? '0' + date.getDate() : date.getDate();
                return year+month+day;
            };
            //获取n天前/后的日期
            let getNDate = function(date,n){
                date.setDate(date.getDate()+n);
                return date;
            };
            //制造数据
            let setWeek = function(date){
                let nowDay = date.getDay()-1;
                date = nowDay < 0 ? getNDate(date,nowDay*+6) : getNDate(date,nowDay*-1);
                _this.currentFirstDate = new Date(date);
                _this.segmentStr = [formatDate(date),formatDate(getNDate(date,6))]
                _this.changeSegment(_this.segmentStr); //数据回调
            };
            //对外方法
            let Export = {}; 
            Export.init = function(now){
                setWeek(now);
            };
            Export.pre = function(){
                setWeek(getNDate(_this.currentFirstDate,-7));
            };
            Export.next = function(){
                setWeek(getNDate(_this.currentFirstDate,7));
            };
            return Export;
        },
        //对比日期
        compareDate (d1,d2) {
            let newD = new Date(d2);
            return new Date(d1) >= new Date(newD.getFullYear(),newD.getMonth(),newD.getDate());
        }
    }
}
</script>

<style scoped>
</style>
