<template>
    <div>
        <div class="calendar">
            <div class="calendar__head">
                <button @click="prevMonth()"> - </button>
                {{ monthes[language][month] }} {{ year }}
                <button @click="nextMonth()"> + </button>
            </div>
            <div class="calendar__weeks">
                <div v-for="week_day in weeks[language]" :key="week_day" class="calendar__cell">
                    {{ week_day }}
                </div>
            </div>
            <div class="calendar__body">
                <div class="calendar_day calendar__cell" v-for="day in skipBefore" :key="day" >
                    
                </div>
                <div class="calendar_day calendar__cell" :class="selectedDay === key ? 'calendar__selected' : ''" v-for="(day, key) in days" :key="day" @click="setDay(key)">
                    {{ key + 1 }}
                </div>
            </div>
            <div class="calendar__bottom"> Выбранно: {{ year }} {{ monthes[language][month] }} 
                <span  v-if="selectedDay !== false"> {{ selectedDay + 1 }}</span> </div>
        </div>
        <div class="languages">
            <span>{{ languageQuestion[language] }}:</span>
            <button @click="setLang('en')"> {{ eng[language] }} </button>
            <button @click="setLang('rus')">{{ rus[language] }} </button>
        </div>
    </div>
</template>

<script>

export default {
    props: {
        date: {
            default: false
        }
    },
    data: () => ({
        year: 0,
        month: 0,
        day: 0,
        currentList: false,
        language: "rus",
        languageQuestion: {
            rus: "Выберите язык",
            en: "Select language"
        },
        eng:{
            rus: "Английский",
            en: "English"
        },
        rus:{
            rus: "Русский",
            en: "Russian"
        },
        monthes: {
            rus:  [
                'Январь', 'Февраль', 'Март', "Апрель", 
                "Май", "Июнь", "Июль", "Август", 
                "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"
            ],
            en: [
                "January", "February", "March", "April", 
                "May", "June","July", "August", 
                "September", "October", "November", "December"
            ]
        },
        weeks: {
            rus: [
                'Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Суб', 'Вос'
            ],
            en: [
                 'Mon', 'Tue', 'Wed', 'Th', 'Fr', 'Sat', 'Sun'
            ]
        },
        line: [],
        currentFirstDay: [],
        currentLastDay: [],
        days: [],
        selectedDay: false
    }),
    methods: {
        setLang(lang){
            this.language = lang;
        },
        setDay(key){
            this.selectedDay = key;
        },
        prevMonth(){
            this.selectedDay = false;
            if(this.month === 0){
                this.month = 11;
                this.year--;
                this.setList();
                return;
            }

            this.month--;
            this.setList();
            return;
        },
        nextMonth(){
            this.selectedDay = false;
            if(this.month === 11){
                this.month = 0
                this.year++;
                this.setList();
                return;
            }

            this.month++;
            this.setList();
            return;
        },
        setList(){
            this.currentList = new Date(this.year, this.month, this.day);
            this.currentLastDay = new Date(this.currentList.getFullYear(), this.currentList.getMonth() + 1, 0);
            this.currentFirstDay = new Date(this.currentList.getFullYear(), this.currentList.getMonth(), 1);
            let weekDay = this.currentFirstDay.getDay();

            if(!weekDay){
                weekDay = 7;
            }

            this.skipBefore =  [...Array(weekDay - 1).keys()];
            this.days = [...Array(this.currentLastDay.getDate())];

            
        }
    },
    mounted(){
        let date = '';
        if(this.date){
            date = new Date(this.date);
        }else{
            date = new Date();
        }
        this.year = date.getFullYear();
        this.month = date.getMonth();
        this.day = date.getDate();
        this.setList();

  
        this.selectedDay =  this.day - 1;
    }
}
</script>

<style scoped>
    .calendar{
        margin: auto;
        width: 280px;
     
        border: 1px solid #000000
    }
    .calendar__head{
        text-align: center;
        height: 30px;
        display: flex;
        align-items: center;
        justify-content: space-around;
        border-bottom: 1px solid #000000;
    }
    .calendar__weeks{
        display: flex;
        width: 100%;
  
    }
    .calendar__cell{
        display: flex;
        justify-content: center;
        align-items: center;
        height: 30px;
        width: 40px;
    }
    .calendar__body{
        display: flex;
        min-height: 180px;
         flex-wrap: wrap;
    }
    .calendar__selected{
        background-color: rgb(124, 143, 228);
    }
    .calendar__bottom{
        display: flex;
        width: 100%;
        height: 30px;
        justify-content: center;
        align-items: center;
        border-top: 1px solid #000000;
    }
    .calendar__bottom span {
        padding-left: 5px;
    }
    .languages{
        display: flex;
        justify-content: center;
        padding-top: 25px;
    }
    .languages button{
        margin-left: 10px;
    }
</style>
