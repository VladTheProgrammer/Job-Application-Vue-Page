<template>
  <v-container style="padding: 25px;">
    <v-row class="margin-standard">
      <!--Title-->
      <v-col cols="12">
        <h1 class="page-title">Проведение ТО и мелкий ремонт</h1>
      </v-col>
    </v-row>
    <!--Tabs-->
    <v-tabs density="compact" hide-slider class="margin-standard" height="16px">
      <v-tab class="tab" selected-class="tab-selected">Общее</v-tab>
      <v-tab class="tab" selected-class="tab-selected">Товар списания</v-tab>
      <v-tab class="tab" selected-class="tab-selected">Доп. расходы</v-tab>
    </v-tabs>
    <!--Add/Upload/Download Rows Buttons-->
    <v-row class="margin-standard">
      <v-col cols="12">
        <v-card class="white-card" elevation="2">
          <v-btn color="#2f8cff" @click="addItem" flat
            style="font-family: MyriadPro; font-size: 14px; font-weight: normal; font-stretch: normal; font-style: normal; line-height: normal; letter-spacing: normal; margin: 20px 0px 20px 25px; padding: 10px 15px 10px 15px;"
            class="text-white">
            <v-icon left color="#1253a2">mdi-plus</v-icon>Добавить строку
          </v-btn>
          <v-btn color="#2f8cff" @click="retrieveTable" flat
            style="font-family: MyriadPro; font-size: 14px; font-weight: normal; font-stretch: normal; font-style: normal; line-height: normal; letter-spacing: normal; margin: 20px 0px 20px 25px; padding: 10px 15px 10px 15px;"
            class="text-white">
            загрузить таблицу
          </v-btn>
          <v-btn color="#2f8cff" @click="uploadTable" flat
            style="font-family: MyriadPro; font-size: 14px; font-weight: normal; font-stretch: normal; font-style: normal; line-height: normal; letter-spacing: normal; margin: 20px 0px 20px 25px; padding: 10px 15px 10px 15px;"
            class="text-white">
            отправить таблицу
          </v-btn>
        </v-card>
      </v-col>
    </v-row>
    <!--Data Table Section-->
    <v-row no-gutters>
      <v-col cols="12">
        <v-card class="white-card" elevation="2" style="padding: 9px 15px 9px 15px;">
          <v-row style="padding-bottom: 9px;">
            <v-col cols="12" class="text-end">
              <!--Cog Menu-->
              <v-menu location="bottom" :close-on-content-click="false">
                <template v-slot:activator="{ props }">
                  <v-icon class="cog-icon" color="#a6b7d4" size="15" v-bind="props">mdi-cog</v-icon>
                </template>
                <v-list>
                  <v-list-item v-for="(header, index) in headers" :key="index">
                    <v-list-item-title>
                      <v-checkbox v-model="header.visible" color="primary" hide-details single-line
                        :label="header.title"></v-checkbox>
                    </v-list-item-title>
                  </v-list-item>
                </v-list>
              </v-menu>
            </v-col>
          </v-row>
          <v-divider />
          <!--Data Table-->
          <v-data-table :headers="visibleHeaders" :items="items" :items-per-page="-1">
            <!--Maintains no pagination controls-->
            <template v-slot:bottom></template>
          </v-data-table>
          <!--Totals Cards-->
          <v-row justify="end">
            <v-col cols="6" offset="6" class="d-flex flex-column align-end">
              <v-card color="#fbfcfd" variant="flat" class="card-grey" width="304" style="margin-bottom: 5px;">
                <v-row justify="space-around" dense>
                  <v-col cols="12" class=" d-flex justify-space-between">
                    <a class="text-final-quantity-light">Sum</a>
                    <a class="text-final-quantity-dark">{{ sum }}</a>
                  </v-col>

                  <v-col cols="12" class=" d-flex justify-space-between">
                    <a class="text-final-quantity-light">Quantity</a>
                    <a class="text-final-quantity-dark">{{ totalQuantity }}</a>
                  </v-col>

                  <v-col cols="12" class=" d-flex justify-space-between">
                    <a class="text-final-quantity-light">Order Weight</a>
                    <a class="text-final-quantity-dark">{{ orderWeight }}</a>
                  </v-col>
                </v-row>
              </v-card>
              <v-card color="#fbfcfd" variant="flat" class="card-grey" width="304">
                <v-row>
                  <v-col cols="12" class=" d-flex justify-space-between">
                    <a class="text-final-quantity-dark">Total Sum</a>
                    <a class="text-final-quantity-dark font-weight-bold">{{ sum }}</a>
                  </v-col>
                </v-row>
              </v-card>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import { ref, computed } from 'vue';
//Headers Data, reactive and can be imported from server
const headers = ref([
  { title: 'Number', value: 'number', visible: true },
  { title: 'Settings', value: 'settings', visible: true },
  { title: 'Unit Weight', value: 'unitWeight', visible: true },
  { title: 'Price', value: 'price', visible: true },
  { title: 'Quantity', value: 'quantity', visible: true },
  { title: 'Name', value: 'name', visible: true },
  { title: 'Total', value: 'total', visible: true },
]);
//Table Body data, reactive and can be imported from server
const items = ref([
  { number: 1, settings: 'A', unitWeight: 10, price: 100, quantity: 1, name: 'Item 1', total: 100 },
  { number: 2, settings: 'B', unitWeight: 15, price: 200, quantity: 1, name: 'Item 2', total: 200 },
  { number: 3, settings: 'C', unitWeight: 20, price: 300, quantity: 1, name: 'Item 3', total: 300 },
  { number: 4, settings: 'D', unitWeight: 25, price: 400, quantity: 1, name: 'Item 4', total: 400 },
]);
//Function responsible for adding a new table row
const addItem = () => {
  const newItem = {
    number: items.value.length + 1,
    settings: 'New',
    unitWeight: 30,
    price: 500,
    quantity: 1,
    name: 'New Item',
    total: 500,
  };
  items.value.push(newItem);
};
//Totals properties are computed so that the "Totals" cards can share data with the table
const sum = computed(() => items.value.reduce((acc, item) => acc + item.total, 0));
const totalQuantity = computed(() => items.value.reduce((acc, item) => acc + item.quantity, 0));
const orderWeight = computed(() => items.value.reduce((acc, item) => acc + item.unitWeight * item.quantity, 0));
//Visible headers based on user selection
const visibleHeaders = computed(() =>
  headers.value.filter(header => header.visible)
);
//Functionality to upload the table to the server
async function uploadTable() {
  try {
    const response = await fetch('http://example.com/upload', {
      method: 'POST',
      body: JSON.stringify({ headers: headers.value, items: items.value }),
      headers: { 'Content-Type': 'application/json' },
    });
    if (!response.ok) {
      throw new Error('Unable to upload table data');
    }
    console.log('Table data uploaded successfully');
  } catch (error) {
    console.error(error.message);
  }
}
//Function to download the data from the server
async function retrieveTable() {
  try {
    const response = await fetch('http://example.com/tabledata');
    if (!response.ok) {
      throw new Error('Unable to retrieve table data');
    }
    const { headers: retrievedHeaders, items: retrievedItems } = await response.json();
    headers.value = retrievedHeaders;
    items.value = retrievedItems;
    console.log('Table data retrieved successfully');
  } catch (error) {
    console.error(error.message);
  }
}
</script>

<style scoped>
.table-header {
  color: rgba(0, 0, 0, 1) !important;
}

.v-table {
  margin-bottom: 15px;
}

.card-grey {
  border: solid 1px #eeeff1;
  padding: 15px;
}

.text-final-quantity-light {
  font-family: MyriadPro;
  font-size: 14px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: normal;
  letter-spacing: normal;
  color: #8f8f8f;
}

.text-final-quantity-dark {
  font-family: MyriadPro;
  font-size: 14px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: normal;
  letter-spacing: normal;
  text-align: right;
  color: #000;
}

.margin-standard {
  margin-bottom: 25px;
}

.page-title {
  width: 433px;
  height: 31px;
  font-family: MyriadPro;
  font-size: 30px;
  font-weight: normal;
  font-stretch: normal;
  font-style: normal;
  line-height: normal;
  letter-spacing: normal;
  color: #000;
}

.tab {
  color: #1253a2;
}

.tab-selected {
  color: black;
  font-weight: bold;
}

.icon-cog {
  color: #a6b7d4;
}
</style>
