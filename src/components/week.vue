<template>
    <div class="time_bucket">
        <div @touchstart="dataSegment().pre()">上周</div>
        <div>{{ segmentStr[0] }} ~ {{ segmentStr[1] }}</div>
        <div :class="{'none':compareDate(segmentStr[1],new Date())}" @touchstart="dataSegment().next()">下周</div>
        <!-- 这里比较如果时间段最大日期超过或等于当天日期时，禁止next点击 -->
    </div>
</template>
<script>
export default {
    data () {
        return {
            segmentStr : ""
        }
    },
    created (){
        this.dataSegment().init(new Date()); //初始化
    },
    props: {
        changeSegment: { type: Function }
    },
    methods : {
        dataSegment () {  //获取时间段数据
            let _this = this;
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
            let setWeek = function(date){
                let week = date.getDay()-1;
                date = getNDate(date,week*-1);
                _this.currentFirstDate = new Date(date);
                _this.segmentStr = [formatDate(date),formatDate(getNDate(date,6))]
                _this.changeSegment(_this.segmentStr); //数据回调
            };
            let Export = {};  //对外接口
            Export.init = function(now){
                setWeek(now);
            };
            Export.pre = function(){
                let  tem = getNDate(_this.currentFirstDate,-7)
                console.log(tem)
                setWeek(tem);
            };
            Export.next = function(){
                setWeek(getNDate(_this.currentFirstDate,7));
            };
            return Export;
        },
        //对比日期
        compareDate (d1,d2) {
            return new Date(d1) >= new Date(d2);
        }
    }
}
</script>

<style scoped>
</style>
