<template>
  <div id="app">
    <div class="timeline" />
    <div v-for="purchase in infoList" :key="purchase.transactionId">
      <Item
        :date="purchase.purchaseDate"
        :time="purchase.purchaseTime"
        :location="purchase.purchaseLocation"
        :totalPrice="purchase.purchasePrice"
        :itemsDetails="purchase.items"
        class="card"
      />
    </div>
  </div>
</template>

<script>
import Item from "./components/item.vue";

export default {
  name: "App",
  data() {
    return {
      infoList: []
    };
  },
  components: {
    Item
  },
  methods: {
    findObject(array, keyValue) {
      return array.find(object => object.key === keyValue).value;
    },
    getInfoList: function() {
      fetch("https://storage.googleapis.com/dito-questions/events.json")
        .then(result => result.json())
        .then(list => {
          const eventList = list.events;
          const buyEventList = eventList.filter(
            eventItem => eventItem.event === "comprou"
          );
          const result = [];
          buyEventList.map(buyEvent => {
            const transactionId = this.findObject(
              buyEvent.custom_data,
              "transaction_id"
            );
            const totalPrice = buyEvent.revenue;
            const location = this.findObject(
              buyEvent.custom_data,
              "store_name"
            );
            const timeInfo = new Date(buyEvent.timestamp);
            const dateInfo = timeInfo.toLocaleDateString("pt-BR");
            const hour = timeInfo.getHours();
            const minute = timeInfo.getMinutes();
            const time = hour + ":" + minute;
            const items = [];

            const productInfo = eventList.filter(
              buyEvent =>
                buyEvent.event === "comprou-produto" &&
                buyEvent.custom_data.find(x => x.key === "transaction_id")
                  .value === transactionId
            );
            productInfo.map(item => {
              const itemData = item.custom_data;
              const itemName = this.findObject(itemData, "product_name");
              const itemPrice = this.findObject(itemData, "product_price");
              const itemDetails = {
                name: itemName,
                price: itemPrice
              };
              items.push(itemDetails);
            });

            const transactionData = {
              transactionId: transactionId,
              purchaseLocation: location,
              purchaseDate: dateInfo,
              purchaseTime: time,
              purchasePrice: totalPrice,
              items: items
            };
            result.push(transactionData);
            return result;
          });
          this.infoList = result;
        });
    }
  },
  mounted() {
    this.getInfoList();
  }
};
</script>

<style>
.timeline::after {
  content: "";
  position: absolute;
  width: 3px;
  background-color: #cecece;
  top: 0;
  bottom: 0;
  left: 210px;
  height: 140%;
}
body {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #e5ecf0;
}
.card {
  border-radius: 10px;
  margin: 60px 15rem 0 15rem;
  box-shadow: 0px 1px 9px #20202033;
  background-color: #f5f5f5;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #4d4d4d;
  width: 100%;
}
</style>
