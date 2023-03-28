<template>
  <v-container>
    <a-tabs v-model:activeKey="activeKey">
      <a-tab-pane key="1" tab="วัน" force-render>
        <a-row justify="space-between">
          <a-col>
            <a-date-picker
              v-model:value="value1"
              :locale="locale"
              format="DD/MMMM/BBBB"
              style="width: 20rem" />
          </a-col>
          <a-col>
            <a-row>
              <a-col class="mr-10">
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
                <a-tag v-if="total_price >= 0" color="#87d068">{{
                  total_price
                }}</a-tag>
                <a-tag v-else color="#f50">{{ total_price }}</a-tag>
              </a-con>
            </a-row>
          </a-col>
        </a-row>
        <div class="relative overflow-x-auto shadow-md mt-4">
          <table
            class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead
              class="text-xs"
              style="background-color: #1e88e5; color: white">
              <tr>
                <th scope="col" class="px-6 py-3">ลำดับ</th>
                <!-- <th scope="col" class="px-6 py-3">วัน เดือน ปี</th> -->
                <th scope="col" class="px-6 py-3">รายการ</th>
                <th scope="col" class="px-6 py-3">รายรับ (บาท)</th>
                <th scope="col" class="px-6 py-3">รายจ่าย (บาท)</th>
                <th scope="col" class="px-6 py-3">หมายเหตุ</th>
              </tr>
            </thead>
            <tbody>
              <tr
                class="table-row-hover"
                v-for="(item, index) in data_ls"
                :key="index">
                <td class="px-6 py-4">{{ index + 1 }}</td>
                <!-- <td class="px-6 py-4">{{ item.date }}</td> -->
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
                  <div v-else>-</div>
                </td>
                <td class="px-6 py-4">
                  <a-tag color="red" v-if="!item.total_net_price">
                    {{ item.total_price }}
                  </a-tag>
                  <div v-else>-</div>
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
            <a-date-picker
              v-model:value="value2"
              picker="month"
              :locale="locale"
              format="MMMM/BBBB"
              style="width: 20rem" />
          </a-col>
          <a-col>
            <a-row>
              <a-col class="mr-10">
                <a-con>
                  <label>รายรับ </label>
                  <a-tag color="success">{{ sumTotalNetPriceMonth() }}</a-tag>
                </a-con>
                <a-con>
                  <label>| รายจ่าย </label>
                  <a-tag color="error">{{ sumTotalPriceMonth() }}</a-tag>
                </a-con>
              </a-col>
              <a-con>
                <label> กำไร/ขาดทุน </label>
                <a-tag v-if="total_price >= 0" color="#87d068">{{
                  total_price
                }}</a-tag>
                <a-tag v-else color="#f50">{{ sumRevenueMonth() }}</a-tag>
              </a-con>
            </a-row>
          </a-col>
        </a-row>
        <div class="relative overflow-x-auto shadow-md mt-4">
          <table
            class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead
              class="text-xs"
              style="background-color: #1e88e5; color: white">
              <tr>
                <th scope="col" class="px-6 py-3">วัน</th>
                <th scope="col" class="px-6 py-3">รายรับ (บาท)</th>
                <th scope="col" class="px-6 py-3">รายจ่าย (บาท)</th>
                <th scope="col" class="px-6 py-3">หมายเหตุ</th>
              </tr>
            </thead>
            <tbody>
              <tr
                class="table-row-hover"
                v-for="(item, index) in month_ls"
                :key="index">
                <td class="px-6 py-4">{{ index }}</td>
                <td class="px-6 py-4">
                  <a-tag color="green">
                    {{ item.total_net_price }}
                  </a-tag>
                </td>
                <td class="px-6 py-4">
                  <a-tag color="red">
                    {{ item.total_price }}
                  </a-tag>
                </td>
                <td class="px-6 py-4">
                  <a @click.stop.prevent="sendToDay(index)"
                    >รายละเอียดเพิ่มเติม</a
                  >
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </a-tab-pane>

      <a-tab-pane key="3" tab="ปี" force-render>
        <a-row justify="space-between">
          <a-col>
            <a-date-picker
              v-model:value="value3"
              picker="year"
              :locale="locale"
              format="BBBB"
              style="width: 20rem" />
          </a-col>
          <a-col>
            <a-row>
              <a-col class="mr-10">
                <a-con>
                  <label>รายรับ </label>
                  <a-tag color="success">{{ sumTotalNetPriceYear() }}</a-tag>
                </a-con>
                <a-con>
                  <label>| รายจ่าย </label>
                  <a-tag color="error">{{ sumTotalPriceYear() }}</a-tag>
                </a-con>
              </a-col>
              <a-con>
                <label> กำไร/ขาดทุน </label>
                <a-tag v-if="sumRevenueYear() >= 0" color="#87d068">{{
                  sumRevenueYear()
                }}</a-tag>
                <a-tag v-else color="#f50">{{ sumRevenueYear() }}</a-tag>
              </a-con>
            </a-row>
          </a-col>
        </a-row>
        <div class="relative overflow-x-auto shadow-md mt-4">
          <table
            class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead
              class="text-xs"
              style="background-color: #1e88e5; color: white">
              <tr>
                <th scope="col" class="px-6 py-3">เดือน</th>
                <th scope="col" class="px-6 py-3">รายรับ (บาท)</th>
                <th scope="col" class="px-6 py-3">รายจ่าย (บาท)</th>
                <th scope="col" class="px-6 py-3">หมายเหตุ</th>
              </tr>
            </thead>
            <tbody>
              <tr
                class="table-row-hover"
                v-for="(item, index) in years_ls"
                :key="index">
                <td class="px-6 py-4">{{ index }}</td>
                <td class="px-6 py-4">
                  <a-tag color="green">
                    {{ item.total_net_price }}
                  </a-tag>
                </td>
                <td class="px-6 py-4">
                  <a-tag color="red">
                    {{ item.total_price }}
                  </a-tag>
                </td>
                <td class="px-6 py-4">
                  <a @click.stop.prevent="sendToMonth(index)"
                    >รายละเอียดเพิ่มเติม</a
                  >
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </a-tab-pane>

      <a-tab-pane key="4" tab="วันเดือน" force-render>
        <a-row justify="space-between">
          <a-col>
            <a-date-picker
              v-model:value="value4"
              picker="month"
              :locale="locale"
              format="MMMM/BBBB"
              style="width: 20rem" />
          </a-col>
          <a-col>
            <a-row>
              <a-col class="mr-10">
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
                <a-tag v-if="total_price >= 0" color="#87d068">{{
                  total_price
                }}</a-tag>
                <a-tag v-else color="#f50">{{ total_price }}</a-tag>
              </a-con>
            </a-row>
          </a-col>
        </a-row>
        <div class="relative overflow-x-auto shadow-md mt-4">
          <table
            class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead
              class="text-xs"
              style="background-color: #1e88e5; color: white">
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
              <tr
                class="table-row-hover"
                v-for="(item, index) in data_ls"
                :key="index">
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
                  <div v-else>-</div>
                </td>
                <td class="px-6 py-4">
                  <a-tag color="red" v-if="!item.total_net_price">
                    {{ item.total_price }}
                  </a-tag>
                  <div v-else>-</div>
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
      value1: dayjs(),
      value2: dayjs(),
      value3: dayjs(),
      value4: dayjs(),
      data_ls: [] as any,
      data_ls2: [] as any,
      month_ls: [] as any,
      years_ls: [] as any,
      month: [] as any,
      tours: [] as any,
      activeKey: "1",
      total_price: 0,
      total_quo: 0,
      total_esti: 0,
    };
  },
  watch: {
    activeKey(newValue) {
      if (newValue == "2") {
        this.month_selected();
      } else if (newValue == "3") {
        this.year_selected();
      }
    },
    value1() {
      if (this.value1 == null) {
        this.data_ls = this.data_ls2;
      } else {
        this.data_ls = this.data_ls2.filter(
          (x: any) => x.date == dayjs(this.value1).format("DD/MM/BBBB")
        );
      }
      this.data_ls.sort((a: any, b: any) => {
        return a.date < b.date ? -1 : 1;
      });
      this.calculateTotalPrice();
    },
    value2() {
      this.month_selected();
    },
    value3() {
      this.year_selected();
    },
    value4() {
      if (this.value4 == null) {
        this.data_ls = this.data_ls2;
      } else {
        const date = dayjs(this.value4).format("MM/BBBB");
        this.data_ls = this.data_ls2.filter((x: any) => x.date.includes(date));
      }
      this.data_ls.sort((a: any, b: any) => {
        return a.date < b.date ? -1 : 1;
      });
      this.calculateTotalPrice();
    },
  },
  async mounted() {
    const d1 = await read_all_data("estimates");
    const d2 = await read_all_data("quotations");
    this.tours = await read_all_data("tours");
    this.data_ls = d1.concat(d2).sort((a: any, b: any) => {
      return a.date < b.date ? -1 : 1;
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
      this.total_price = this.sumTotalNetPrice() - this.sumTotalPrice();
    },
    month_selected() {
      const data = this.data_ls2.filter((x: any) =>
        x.date.includes(dayjs(this.value2).format("MM/BBBB"))
      );
      console.log(data);
      let result = data.reduce((accumulator: any, currentItem: any) => {
        let currentDate = currentItem.date;
        let currentTotalPrice = currentItem.total_price || 0;
        let currentTotalNetPrice = currentItem.total_net_price || 0;
        if (accumulator.hasOwnProperty(currentDate)) {
          accumulator[currentDate].total_net_price += currentTotalNetPrice;
          if (currentTotalNetPrice === 0) {
            accumulator[currentDate].total_price += currentTotalPrice;
          }
        } else {
          accumulator[currentDate] = {
            total_price: currentTotalPrice,
            total_net_price: currentTotalNetPrice,
          };
        }
        return accumulator;
      }, {});

      console.log(result);
      this.month_ls = result;
    },
    sumTotalNetPriceMonth() {
      let sum = 0;
      Object.keys(this.month_ls).forEach((x: any) => {
        sum += this.month_ls[x].total_net_price;
      });
      return sum;
    },
    sumTotalPriceMonth() {
      let sum = 0;
      Object.keys(this.month_ls).forEach((x: any) => {
        sum += this.month_ls[x].total_price;
      });
      return sum;
    },
    sumRevenueMonth() {
      return this.sumTotalNetPriceMonth() - this.sumTotalPriceMonth();
    },
    year_selected() {
      const data = this.data_ls2.filter((x: any) =>
        x.date.includes(dayjs(this.value3).format("BBBB"))
      );
      console.log(data);
      let result = data.reduce((accumulator: any, currentItem: any) => {
        let currentDate = currentItem.date.slice(3);
        let currentTotalPrice = currentItem.total_price || 0;
        let currentTotalNetPrice = currentItem.total_net_price || 0;
        if (accumulator.hasOwnProperty(currentDate)) {
          accumulator[currentDate].total_net_price += currentTotalNetPrice;
          if (currentTotalNetPrice === 0) {
            accumulator[currentDate].total_price += currentTotalPrice;
          }
        } else {
          accumulator[currentDate] = {
            total_price: currentTotalPrice,
            total_net_price: currentTotalNetPrice,
          };
        }
        return accumulator;
      }, {});

      this.years_ls = result;
    },
    sumTotalNetPriceYear() {
      let sum = 0;
      Object.keys(this.years_ls).forEach((x: any) => {
        sum += this.years_ls[x].total_net_price;
      });
      return sum;
    },
    sumTotalPriceYear() {
      let sum = 0;
      Object.keys(this.years_ls).forEach((x: any) => {
        sum += this.years_ls[x].total_price;
      });
      return sum;
    },
    sumRevenueYear() {
      return this.sumTotalNetPriceYear() - this.sumTotalPriceYear();
    },
    sendToMonth(date: any) {
      this.activeKey = "2";
      this.value2 = dayjs(date, "MM/BBBB");
    },
    sendToDay(date: any) {
      this.activeKey = "1";
      this.value1 = dayjs(date, "DD/MM/BBBB");
    },
  },
});
</script>
