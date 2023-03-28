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
                <a-tag v-if="total_price >= 0" color="#87d068">{{ total_price }}</a-tag>
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
                <th scope="col" class="px-6 py-3">วัน</th>
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

      <a-tab-pane key="3" tab="ปี" force-render>
        <a-row justify="space-between">
          <a-col>
            <a-date-picker
              v-model:value="value4"
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
                <th scope="col" class="px-6 py-3">เดือน</th>
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
      if (this.value1 == null) {
        this.data_ls = this.data_ls2;
      } else {
        this.data_ls = this.data_ls2.filter(
          (x: any) =>
            x.date.split("/")[0] ==
            dayjs(this.value1).format("DD/MM/BBBB").split("/")[0]
        );
      }
      this.data_ls.sort((a: any, b: any) => {
        return a.date < b.date ? -1 : 1;
      });
      this.calculateTotalPrice();
    },
    value2() {
      if (this.value2 == null) {
        this.data_ls = this.data_ls2;
      } else {
      const date = dayjs(this.value2).format("MM/BBBB");
      this.data_ls = this.data_ls2.filter((x: any) => x.date.includes(date));
      }
      this.data_ls.sort((a: any, b: any) => {
        return a.date < b.date ? -1 : 1;
      });
      this.calculateTotalPrice();
    },
    value3() {
      if (this.value3 == null) {
        this.data_ls = this.data_ls2;
      } else {
        const date = dayjs(this.value3).format("BBBB");
        this.data_ls = this.data_ls2.filter((x: any) => x.date.includes(date));
      }
      this.data_ls.sort((a: any, b: any) => {
        return a.date < b.date ? -1 : 1;
      });
      this.calculateTotalPrice();
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
  },
});
</script>
