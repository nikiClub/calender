<template>
    <div class="calender-content">
        <div class="title">
            <div class="prev" @click="prev">上一月</div>
            <div class="txt">{{year}} 年 {{month}} 月</div>
            <div class='next' @click="next">下一月</div>
        </div>
        <div class="date-wrap">
            <ul class="week">
                <li>日</li>
                <li>一</li>
                <li>二</li>
                <li>三</li>
                <li>四</li>
                <li>五</li>
                <li>六</li>
            </ul>
            <ul class="day">
                <li v-for="item in days" @click="itemClick(item)" :key="item.date" :class="[item.selected?'active':'']">
                    {{item.name}}
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
    export default {
        name: "my-calender",
        data() {
            return {
                year: "",
                month: "",
                days: [
                    // {name: "1", date: "2020-6-15", current: false, reservation: false}
                ],
            }
        },
        mounted() {
            this.init();
        },
        methods: {
            // 初始化
            init() {
                let currentDate = new Date();
                this.year = currentDate.getFullYear();
                this.month = currentDate.getMonth() + 1;

                this.createDaysArr();
            },


            // 获取显示日历信息
            createDaysArr() {
                this.days.length = 0;
                // 生成当前月日期
                for (let i = 1; i <= this.getDays(); i++) {
                    this.days.push({
                        name: i,
                        date: this.year + "-" + this.month + "-" + i,
                        year: this.year,
                        month: this.month,
                        day: i,
                        current: false,
                        reservation: false,
                        selected: false
                    })
                }

                for (let i = 1; i <= 42 - this.getWeek() - this.getDays(); i++) {
                    this.days.push({
                        name: i,
                        date: this.year + "-" + (this.month + 1) + "-" + i,
                        year: this.year,
                        month: this.month + 1,
                        day: i,
                        current: "",
                        reservation: false,
                        selected: false
                    })
                }

                for (let i = 0; i < this.getWeek(); i++) {
                    let prevMonthDays = this.getDays(this.year, this.month - 1);
                    this.days.unshift({
                        name: prevMonthDays - i,
                        date: this.year + "-" + (this.month - 1) + "-" + (prevMonthDays - i),
                        year: this.year,
                        month: this.month - 1,
                        day: prevMonthDays - i,
                        current: false,
                        c: false,
                        selected: false
                    })
                }
            },

            // 获取月份是多少天
            getDays(year, month) {
                if (year != undefined && month != undefined) {
                    return new Date(year, month, 0).getDate();
                } else {
                    return new Date(this.year, this.month, 0).getDate();
                }
            },

            // 获取1日是周几
            getWeek() {
                return new Date(this.year, this.month - 1, 1).getDay();
            },

            // 上一个月
            prev() {
                if (this.month > 1) {
                    this.month = this.month - 1;
                    this.createDaysArr();
                } else if (this.year > 1970) {
                    this.month = 12;
                    this.year = this.year - 1;
                    this.createDaysArr();
                }
            },

            // 下一个月
            next() {
                if (this.month < 12) {
                    this.month = this.month + 1;
                    this.createDaysArr();
                } else {
                    this.month = 1;
                    this.year = this.year + 1;
                    this.createDaysArr();
                }
            },
            // 选中日期
            itemClick(item) {
                console.log(item.date);
                console.log(item);

                this.$set(item, 'selected', !item.selected);
            }
        }
    }
</script>

<style lang="scss" scoped>
    .calender-content {
        width: 100%;
        height: 100%;

        .title {
            box-sizing: border-box;
            width: 100%;
            height: 1rem;
            line-height: 1rem;
            font-size: 0.3rem;
            text-align: center;
            /*border: 1px solid #d3d4d4;*/

            .prev {
                float: left;
                width: 20%;
            }

            .txt {
                float: left;
                width: 60%;
            }
            .next {
                float: right;
                width: 20%;
            }
        }

        .date-wrap {
            width: 100%;
            height: 6rem;

            .week {
                width: 100%;
                height: 1rem;
                line-height: 1rem;
                text-align: center;
                font-size: 0.26rem;
                color: #000;
                border-top: 1px solid #d3d4d4;

                li {
                    box-sizing: border-box;
                    float: left;
                    width: 14.28%;
                    height: 100%;
                    border-right: 1px solid #d3d4d4;

                    &:last-child {
                        border-right: none;
                    }
                }

            }

            .day {
                width: 100%;
                height: 5rem;
                font-size: 0.26rem;
                color: #000;
                border-top: 1px solid #d3d4d4;

                li {
                    box-sizing: border-box;
                    float: left;
                    width: 14.28%;
                    height: 1rem;
                    line-height: 1rem;
                    text-align: center;
                    font-size: 0.26rem;
                    border-right: 1px solid #d3d4d4;
                    border-bottom: 1px solid #d3d4d4;

                    &:last-child {
                        border-right: none;
                    }
                }

                li.active {
                    /*border: 2px solid #fb0;*/
                    background: #fb0;;
                    color: #fff;
                }

                .noReservation {
                    background: #d8e4f2;
                }

                .okReservation {
                    background: #efcdd1;
                }
            }
        }
    }
</style>