<template>
  <div
    style="
      width: 100%;
      height: 100vh;
      padding: 50px;
      box-sizing: border-box;
      overflow: hidden;
      display: flex;
      align-items: center;
      justify-content: center;
    "
  >
    <div style="width: 100%; height: 100%">
      <SweetVueCalendar :config="config" long-week />
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
        formatDate:'DD/MM/YYYY',
        formatTime:'HH:MM:mm',
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
