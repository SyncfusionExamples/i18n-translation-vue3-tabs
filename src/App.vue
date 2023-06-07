<script>
import { watch } from "vue";
import { createApp } from "vue";

import {
  GridComponent,
  ColumnsDirective,
  ColumnDirective,
} from "@syncfusion/ej2-vue-grids";
import {
  TabComponent
} from "@syncfusion/ej2-vue-navigations";
import { useI18n } from "vue-i18n";
import NewTab from "./components/NewTab.vue";
import { i18n } from "./main";

export default {
  name: "App",
  components: {
    "ejs-tab": TabComponent,
    "ejs-grid": GridComponent,
    "e-columns": ColumnsDirective,
    "e-column": ColumnDirective,
  },
  data() {
    return {
      modules: [i18n],
      i: 0,
      numb: 1,
      data: [
        {
          OrderID: 10248,
          CustomerID: "VINET",
          EmployeeID: 5,
          OrderDate: new Date(8364186e5),
          ShipName: "Vins et alcools Chevalier",
          ShipCity: "Reims",
          ShipAddress: "59 rue de l Abbaye",
          ShipRegion: "CJ",
          ShipPostalCode: "51100",
          ShipCountry: "France",
          Freight: 32.38,
          Verified: !0,
        },
        {
          OrderID: 10249,
          CustomerID: "TOMSP",
          EmployeeID: 6,
          OrderDate: new Date(836505e6),
          ShipName: "Toms Spezialitäten",
          ShipCity: "Münster",
          ShipAddress: "Luisenstr. 48",
          ShipRegion: "CJ",
          ShipPostalCode: "44087",
          ShipCountry: "Germany",
          Freight: 11.61,
          Verified: !1,
        },
        {
          OrderID: 10250,
          CustomerID: "HANAR",
          EmployeeID: 4,
          OrderDate: new Date(8367642e5),
          ShipName: "Hanari Carnes",
          ShipCity: "Rio de Janeiro",
          ShipAddress: "Rua do Paço, 67",
          ShipRegion: "RJ",
          ShipPostalCode: "05454-876",
          ShipCountry: "Brazil",
          Freight: 65.83,
          Verified: !0,
        },
      ],
    };
  },
  methods: {
    ship() {
      this.numb = this.numb + 1;
      var tabObj = this.$refs.TabInstance.ej2Instances;
      let item = {
        header: { text: "NewTab" + this.i++ },
        content: this.NewTabTemplate,
      };
      let totalItems = document.querySelectorAll("#dynamic_tab .e-toolbar-item")
        .length;
      tabObj.addTab([item], totalItems - 2);
    },
    rowSelected: function () {
      this.ship(); // Get the selected records.
    },
    NewTabTemplate: function () {
      let _this = this;
      let baba = createApp().component("NewTabTemplate", {
        data() {
          return {
            num: _this.numb,
          };
        },
        template: `<NewTab :num="num" />`,
        methods: {},
        mounted() {},
        components: {
          NewTab: NewTab,
        },
      });
      return { template: baba };
    },
    itemCodeTemplate: function () {
      let _this = this;
      let baba = createApp().component("itemCodeTemplate", {
        data: () => ({}),
        template: `dsds<button type="button" class="btn btn-primary" @click="onsubmit">shipCountry</button>`,
        methods: {
          onsubmit() {
            _this.ship();
          },
        },
        mounted() {},
      });

      return { template: baba };
    },
  },
  setup() {
    const { t, locale } = useI18n();

    watch(locale, (newlocale) => {
      localStorage.setItem("locale", newlocale);
    });

    return {
      t,
      locale,
    };
  },
};
</script>

<template>
  <nav>
    <select v-model="locale">
      <option>zh-TW</option>
      <option>en-US</option>
      <option>ja-JP</option>
    </select>
  </nav>
  <p>{{ t("cancel") }}</p>

  <ejs-tab id="dynamic_tab" :plugins="modules" ref="TabInstance">
    <div class="e-tab-header">
      <div>Product List</div>
      <div>Add</div>
    </div>
    <div class="e-content">
      <div>
        {{ t("message") }}
        <ejs-grid :dataSource="data" :rowSelected="rowSelected">
          <e-columns>
            <e-column field="OrderID" width="100" textAlign="Right"></e-column>
            <e-column field="CustomerID" width="100"></e-column>
            <e-column
              field="EmployeeID"
              width="100"
              textAlign="Right"
            ></e-column>
            <e-column
              field="Freight"
              width="100"
              format="C2"
              textAlign="Right"
            ></e-column>
            <e-column
              field="ShipCountry"
              width="100"
              :template="itemCodeTemplate"
            ></e-column>
          </e-columns>
        </ejs-grid>
      </div>
      <div>Second Tab</div>
    </div>
  </ejs-tab>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
