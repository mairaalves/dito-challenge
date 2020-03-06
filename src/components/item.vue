<template>
  <div>
    <Check class="check-icon"/>
    <div class="header">
      <span>
        <Calendar class="icon"/>
        <span>{{date}}</span>
      </span>

      <span>
        <Clock class="icon"/>
        <span>{{time}}</span>
      </span>

      <span>
        <Place class="icon"/>
        <span>{{location}}</span>
      </span>

      <span>
        <Money class="icon"/>
        <span>{{totalPrice.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' })}}</span>
      </span>
    </div>
    <table>
      <thead>
        <th>Produto</th>
        <th>Preço</th>
      </thead>
      <tbody>
        <tr v-for="product in itemsDetails" :key="product.name">
          <td>{{product.name}}</td>
          <td>{{product.price.toLocaleString('pt-BR', { style: 'currency', currency: 'BRL' })}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Calendar from "@/assets/calendar.svg?inline";
import Clock from "@/assets/clock.svg?inline";
import Money from "@/assets/money.svg?inline";
import Place from "@/assets/place.svg?inline";
import Check from "@/assets/checkIcon.svg?inline";


export default {
  name: "item",
  props: {
    date: {
      type: String,
      default: ""
    },
    time: {
      type: String,
      default: ""
    },
    location: {
      type: String,
      default: ""
    },
    totalPrice: {
      type: Number
    },
    itemsDetails: {
      type: Array
    },
    productName: {
      type: String,
      default: "---"
    },
    productPrice: {
      type: String,
      default: "---"
    }
  },
  components: {
    Calendar,
    Clock,
    Money,
		Place,
		Check
  }
};
</script>

<style>
.header {
  display: flex;
  justify-content: space-around;
  align-items: center;
  position: relative;
  background-color: white;
  height: 40px;
  border-radius: 10px 10px 0 0;
  box-shadow: 0px 1px 2px rgba(0, 0, 0, 0.1);
}
.header:after {
  content: "";
  position: absolute;
  width: 0;
  height: 0;
  border-top: 10px solid transparent;
  border-bottom: 15px solid transparent;
  border-right: 17px solid white;
  left: -15px;
  top: 11px;
}
.check-icon{
	position: absolute;
	left: 195.5px;
	height: 32px;
  width: 32px;
	padding-top: 7px;
}
.icon {
  height: 1rem;
  width: 1rem;
  margin-right: 5px;
}
th,
td {
  padding: 10px;
}
table {
  border-radius: 0 0 10px 10px;
  width: 88%;
  margin: auto;
  text-align: left;
  background-color: #f5f5f5;
  border-collapse: collapse;
  overflow-x: auto;
}
tr {
  border-top: #ececece6 solid 2px;
}
</style>