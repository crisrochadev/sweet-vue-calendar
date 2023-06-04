<template>
  <div
    :style="{
      width: '100%',
      height: '100vh',
      padding: '50px',
      boxSizing: 'border-box',
      overflow: 'hidden',
      display: 'flex',
      alignItems: 'center',
      justifyContent: 'center',
    }"
  >
    <div
      :style="{
        width: '100%',
        height: '100%',
        display: 'flex',
        justifyContent: 'center',
        alignItems: 'center',
      }"
    >
      <SweetVueCalendar :config="config" long-week size="medium">
      <template #week="{week}">
        <p :style="{
          backgroundColor:'blueviolet',
          height:'30px',
          display:'flex',
          justifyContent:'center',
          alignItems:'center',
          color:'white',
          fontSize:'8px',
          textTransform:'uppercase',
          fontFamily:'Roboto'
        }">{{ week }}</p>
      </template>
      <template #day="{day}">
        <p :style="{
          backgroundColor:day.currentMonth ? 'blueviolet' : 'silver',
          height:'100%',
          display:'flex',
          justifyContent:'center',
          alignItems:'center',
          color:'white',
          fontSize:'20px',
          textTransform:'uppercase',
          fontFamily:'Roboto'
        }">{{ day.value }}</p>
      </template>
      </SweetVueCalendar>
    </div>
  </div>
</template>
<script>
import SweetVueCalendar from "./components/SweetVueCalendar.vue";

export default {
  components: { SweetVueCalendar },
  data() {
    return {
      config: {
        locale: "pt-br",
        formatDate: "DD/MM/YYYY",
        formatTime: "HH:MM:mm",
        months: [
          "Janeiro",
          "Fevereiro",
          "Março",
          "Abril",
          "Maio",
          "Junho",
          "Julho",
          "Agosto",
          "Setembro",
          "Outubro",
          "Novembro",
          "Dezembro",
        ],
        monthsShort: [
          "Jan",
          "Fev",
          "Mar",
          "Abr",
          "Mai",
          "Jun",
          "Jul",
          "Ago",
          "Set",
          "Out",
          "Nov",
          "Sez",
        ],
        weekdays: [
          "Domingo",
          "Segunda-Feira",
          "Terça-Feira",
          "Quarta-Feira",
          "Quinta_feira",
          "Sexta-Feira",
          "Sábado",
        ],
        weekdaysShort: ["Dom", "Seg", "Ter", "Qua", "Qui", "Sex", "Sab"],
      },
    };
  },
};
</script>
<style lang=""></style>
