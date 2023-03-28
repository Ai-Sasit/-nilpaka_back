<template>
  <v-container>
    <a-tabs v-model:activeKey="activeKey">

      <a-tab-pane key="1" tab="วัน">
        <a-row justify="space-between">
          <a-col>
            <a-date-picker v-model:value="value1" :locale="locale"
              style="width: 20rem" />
          </a-col>
          <a-col>
            <a-row>
              <a-col  class="mr-10">
                <a-con>
                  <label>รายรับ </label>
                  <a-tag color="success">{{ total_quo }}</a-tag>
                </a-con>
                <a-con>
                  <label>| รายจ่าย </label>
                  <a-tag color="error">{{ total_esti }}</a-tag>
                </a-con>
              </a-col>
              <a-con>
                <label> กำไร/ขาดทุน </label>
                <a-tag v-if="total_price >= 0" color="#87d068">{{ total_price }}</a-tag>
                <a-tag v-else color="#f50">{{ total_price }}</a-tag>
              </a-con>
            </a-row>
          </a-col>
        </a-row>
        <div class="relative overflow-x-auto shadow-md mt-4">
          <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead class="text-xs" style="background-color: #1e88e5; color: white">
              <tr>
                <th scope="col" class="px-6 py-3">ลำดับ</th>
                <th scope="col" class="px-6 py-3">วัน เดือน ปี</th>
                <th scope="col" class="px-6 py-3">รายการ</th>
                <th scope="col" class="px-6 py-3">รายรับ (บาท)</th>
                <th scope="col" class="px-6 py-3">รายจ่าย (บาท)</th>
                <th scope="col" class="px-6 py-3">หมายเหตุ</th>
              </tr>
            </thead>
            <tbody>
              <tr class="table-row-hover" v-for="(item, index) in data_ls" :key="index">
                <td class="px-6 py-4">{{ index + 1 }}</td>
                <td class="px-6 py-4">{{ item.date }}</td>
                <td class="px-6 py-4">
                  {{
                    item.objective
                    ? item.objective
                    : get_tour_name(item.tour_id)
                  }}
                </td>
                <td class="px-6 py-4">
                  <a-tag color="green" v-if="item.total_net_price">
                  {{ item.total_net_price }}
                  </a-tag>
                  <div v-else>
                    -
                  </div>
                </td>
                <td class="px-6 py-4">
                  <a-tag color="red" v-if="!item.total_net_price">
                    {{ item.total_price }}
                  </a-tag>
                  <div v-else>
                    -
                  </div>
                </td>
                <td class="px-6 py-4"></td>
              </tr>
            </tbody>
          </table>
        </div>
      </a-tab-pane>

      <a-tab-pane key="2" tab="เดือน" force-render>
        <a-row justify="space-between">
          <a-col>
            <a-date-picker v-model:value="value2" picker="month" :locale="locale"
              style="width: 20rem" />
          </a-col>
          <a-col>
            <a-row>
              <a-col  class="mr-10">
                <a-con>
                  <label>รายรับ </label>
                  <a-tag color="success">{{ total_quo }}</a-tag>
                </a-con>
                <a-con>
                  <label>| รายจ่าย </label>
                  <a-tag color="error">{{ total_esti }}</a-tag>
                </a-con>
              </a-col>
              <a-con>
                <label> กำไร/ขาดทุน </label>
                <a-tag v-if="total_price >= 0" color="#87d068">{{ total_price }}</a-tag>
                <a-tag v-else color="#f50">{{ total_price }}</a-tag>
              </a-con>
            </a-row>
            <!-- <a-input :value="`฿ ${calculateTotalPrice()}`" style="
                    width: 20rem;
                    text-align: center;
                    border: 1px solid tomato;" 
            /> -->
          </a-col>
        </a-row>
        <div class="relative overflow-x-auto shadow-md mt-4">
          <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead class="text-xs" style="background-color: #1e88e5; color: white">
              <tr>
                <th scope="col" class="px-6 py-3">วันที่</th>
                <th scope="col" class="px-6 py-3">รายรับ (บาท)</th>
                <th scope="col" class="px-6 py-3">รายจ่าย (บาท)</th>
                <th scope="col" class="px-6 py-3">หมายเหตุ</th>
              </tr>
            </thead>
            <tbody>
              <tr class="table-row-hover" v-for="(item, index) in data_ls" :key="index">
                <td class="px-6 py-4">{{ item.date }}</td>
                <td class="px-6 py-4">
                  <a-tag color="green" v-if="item.total_net_price">5</a-tag>
                  <div v-else>
                    -
                  </div>
                </td>
                <td class="px-6 py-4">
                  <a-tag color="red" v-if="!item.total_net_price"> 2</a-tag>
                  <div v-else>
                    -
                  </div>
                </td>
                <td class="px-6 py-4">
                  <a @click.stop.prevent="activeKey = '1'">ดูรายละเอียด</a>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </a-tab-pane>

      <a-tab-pane key="3" tab="ปี">
        <a-row justify="space-between">
          <a-col>
            <a-date-picker v-model:value="value3" picker="year" :locale="locale"
              style="width: 20rem" />
          </a-col>
          <a-col>
            <!-- <a-input :value="`฿ ${calculateTotalPrice()}`" style="
                    width: 20rem;
                    text-align: center;
                    border: 1px solid tomato;" 
            /> -->
            <label>{{ total_price }}</label>
          </a-col>
        </a-row>
        <div class="relative overflow-x-auto shadow-md mt-4">
          <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead class="text-xs" style="background-color: #1e88e5; color: white">
              <tr>
                <th scope="col" class="px-6 py-3">เดือนที่</th>
                <th scope="col" class="px-6 py-3">รายรับ (บาท)</th>
                <th scope="col" class="px-6 py-3">รายจ่าย (บาท)</th>
                <th scope="col" class="px-6 py-3">หมายเหตุ</th>
              </tr>
            </thead>
            <tbody>
              <tr class="table-row-hover" v-for="(item, index) in data_ls" :key="index">
                <td class="px-6 py-4">{{ index + 1 }}</td>
                <td class="px-6 py-4">
                  <a-tag color="green" v-if="item.total_net_price"></a-tag>
                </td>
                <td class="px-6 py-4">
                  <a-tag color="red" v-if="!item.total_net_price"> 1</a-tag>
                </td>
                <td class="px-6 py-4">
                  <a @click.stop.prevent="activeKey = '2'">ดูรายละเอียด</a>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </a-tab-pane>

      <a-tab-pane key="4" tab="วันเดือน">
        <a-row justify="space-between">
          <a-col>
            <a-date-picker v-model:value="value1" :locale="locale"
              style="width: 20rem" />
          </a-col>
          <a-col>
            <a-row>
              <a-col  class="mr-10">
                <a-con>
                  <label>รายรับ </label>
                  <a-tag color="success">{{ total_quo }}</a-tag>
                </a-con>
                <a-con>
                  <label>| รายจ่าย </label>
                  <a-tag color="error">{{ total_esti }}</a-tag>
                </a-con>
              </a-col>
              <a-con>
                <label> กำไร/ขาดทุน </label>
                <a-tag v-if="total_price >= 0" color="#87d068">{{ total_price }}</a-tag>
                <a-tag v-else color="#f50">{{ total_price }}</a-tag>
              </a-con>
            </a-row>
          </a-col>
        </a-row>
        <div class="relative overflow-x-auto shadow-md mt-4">
          <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead class="text-xs" style="background-color: #1e88e5; color: white">
              <tr>
                <th scope="col" class="px-6 py-3">ลำดับ</th>
                <th scope="col" class="px-6 py-3">วัน เดือน ปี</th>
                <th scope="col" class="px-6 py-3">รายการ</th>
                <th scope="col" class="px-6 py-3">รายรับ (บาท)</th>
                <th scope="col" class="px-6 py-3">รายจ่าย (บาท)</th>
                <th scope="col" class="px-6 py-3">หมายเหตุ</th>
              </tr>
            </thead>
            <tbody>
              <tr class="table-row-hover" v-for="(item, index) in data_ls" :key="index">
                <td class="px-6 py-4">{{ index + 1 }}</td>
                <td class="px-6 py-4">{{ item.date }}</td>
                <td class="px-6 py-4">
                  {{
                    item.objective
                    ? item.objective
                    : get_tour_name(item.tour_id)
                  }}
                </td>
                <td class="px-6 py-4">
                  <a-tag color="green" v-if="item.total_net_price">
                  {{ item.total_net_price }}
                  </a-tag>
                  <div v-else>
                    -
                  </div>
                </td>
                <td class="px-6 py-4">
                  <a-tag color="red" v-if="!item.total_net_price">
                    {{ item.total_price }}
                  </a-tag>
                  <div v-else>
                    -
                  </div>
                </td>
                <td class="px-6 py-4"></td>
              </tr>
            </tbody>
          </table>
        </div>
      </a-tab-pane>

    </a-tabs>
  </v-container>
</template>
<script lang="ts">
import dayjs from "dayjs";
import { defineComponent } from "vue";
import { read_all_data } from "~~/services/pyapi";
import locale from "ant-design-vue/es/date-picker/locale/th_TH";
import buddhistEra from "dayjs/plugin/buddhistEra";
import { any } from "vue-types";
dayjs.extend(buddhistEra);
export default defineComponent({
  setup() {
    return {
      locale,
    };
  },
  data() {
    return {
      value1: "",
      value2: "",
      value3: "",
      value4: "",
      data_ls: [] as any,
      data_ls2: [] as any,
      month: [] as any,
      tours: [] as any,
      activeKey: "1",
      total_price: 0,
      total_quo: 0,
      total_esti: 0,
    };
  },
  watch: {
    value1() {
      // console.log(this.value1);
      // console.log(dayjs(this.value1).format("DD/MM/BBBB"));
      if(this.value1 == null){
        this.data_ls = this.data_ls2; 
      } else {
        this.data_ls = this.data_ls2.filter((x: any) =>
          x.date.split("/")[0] ==
          dayjs(this.value1).format("DD/MM/BBBB").split("/")[0]
        );
      }
      this.data_ls.sort((a: any, b: any) => {
        return (a.date < b.date) ? -1 : 1;
      });
      console.log("D"+this.data_ls);
      this.calculateTotalPrice();
    },
    value2() {
      console.log(dayjs(this.value2).format("DD/MM/BBBB").split("/").slice(1, 2));
      this.data_ls2.forEach((x: any) => {
        console.log(x.date.split("/").slice(1, 2));
      });

      this.data_ls = this.data_ls2.filter((x: any) =>
        x.date.split("/").slice(1, 2) ==
        dayjs(this.value2).format("DD/MM/BBBB").split("/").slice(1, 2)
      );
      // this.month_price();
      console.log("M"+this.data_ls);
      // this.calculateTotalPrice();
    },
    value3(){
      console.log(this.value3);
      this.data_ls = this.data_ls2.filter(
        (x: any) =>
        x.date.split("/")[2] ==
        dayjs(this.value3).format("DD/MM/BBBB").split("/")[2]
      );
      console.log("Y"+this.data_ls);
      this.calculateTotalPrice();
    },
    value4() {
      console.log(dayjs(this.value4).format("DD/MM/BBBB").split("/").slice(1, 2));
      this.data_ls2.forEach((x: any) => {
        console.log(x.date.split("/").slice(1, 2));
      });

      console.log(dayjs(this.value4).format("DD/MM/BBBB"));
      // console.log(this.data_ls2);
      this.data_ls = this.data_ls2.filter((x: any) =>
        x.date.split("/").slice(1, 2) ==
        dayjs(this.value2).format("DD/MM/BBBB").split("/").slice(1, 2)
      );
      console.log("D"+this.data_ls);
      this.calculateTotalPrice();
    },
  },
  async mounted() {
    const d1 = await read_all_data("estimates");
    const d2 = await read_all_data("quotations");
    this.tours = await read_all_data("tours");
    this.data_ls = (d1.concat(d2)).sort((a: any, b: any) => {
      return (a.date < b.date) ? -1 : 1;
    });
    // this.data_ls.sort((a: any, b: any) => {
    //   return a.date - b.date;
    // });
    this.calculateTotalPrice();
    this.data_ls2 = d1.concat(d2);
  },
  methods: {
    get_tour_name(id: any) {
      const tour: any = this.tours.find((x: any) => x.id === id);
      return tour ? tour.name : "";
    },
    sumTotalPrice() {
      let sum = 0;
      this.data_ls.forEach((x: any) => {
        if (x.total_price && !x.total_net_price) {
          sum += x.total_price;
        }
      });
      this.total_esti = sum;
      return sum;
    },
    sumTotalNetPrice() {
      let sum = 0;
      this.data_ls.forEach((x: any) => {
        if (x.total_net_price) {
          sum += x.total_net_price;
        }
      });
      this.total_quo = sum;
      return sum;
    },
    calculateTotalPrice() {
      this.total_price = this.sumTotalNetPrice() - this.sumTotalPrice()
    },

    month_price() {
      this.month = [],
      this.data_ls.forEach((element: any) => {
        this.month.push( element.date, this.sumNetDay(element.date), this.sumEstiDay(element.data))
      });
      console.log("MP"+this.month)
    },

    sumNetDay( date: any){
      let sum = 0;
      this.data_ls.forEach((x: any) => {
        if (x.date == date && x.total_net_price){
          sum += x.total_net_price
        }
      })
    },
    sumEstiDay( date: any){
      let sum = 0;
      this.data_ls.forEach((x: any) => {
        if (x.date == date && x.total_price){
          sum += x.total_price
        }
      })
    },
  },
});
</script>
