<template>
  <v-container>
    <a-tabs v-model:activeKey="activeKey">
      <a-tab-pane key="3" tab="ปี">
        <a-row justify="space-between"
          ><a-col
            ><a-date-picker
              v-model:value="value2"
              picker="year"
              :locale="locale"
              style="width: 20rem" /></a-col
          ><a-col
            ><a-input
              :value="`฿ ${calculateTotalPrice()}`"
              style="
                width: 20rem;
                text-align: center;
                border: 1px solid tomato;
              " /></a-col
        ></a-row>
        <div class="relative overflow-x-auto shadow-md mt-4">
          <table
            class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead
              class="text-xs"
              style="background-color: #1e88e5; color: white">
              <tr>
                <th scope="col" class="px-6 py-3">เดือนที่</th>
                <th scope="col" class="px-6 py-3">รายรับ (จำนวน)</th>
                <th scope="col" class="px-6 py-3">รายจ่าย (จำนวน)</th>
                <th scope="col" class="px-6 py-3">หมายเหตุ</th>
              </tr>
            </thead>
            <tbody>
              <tr
                class="table-row-hover"
                v-for="(item, index) in data_ls"
                :key="index">
                <td class="px-6 py-4">{{ index + 1 }}</td>
                <td class="px-6 py-4">
                  <a-tag color="green" v-if="item.total_net_price">2</a-tag>
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
        </div></a-tab-pane
      >
      <a-tab-pane key="2" tab="เดือน" force-render
        ><div class="relative overflow-x-auto shadow-md">
          <table
            class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead
              class="text-xs"
              style="background-color: #1e88e5; color: white">
              <tr>
                <th scope="col" class="px-6 py-3">วันที่</th>
                <th scope="col" class="px-6 py-3">รายรับ (จำนวน)</th>
                <th scope="col" class="px-6 py-3">รายจ่าย (จำนวน)</th>
                <th scope="col" class="px-6 py-3">หมายเหตุ</th>
              </tr>
            </thead>
            <tbody>
              <tr
                class="table-row-hover"
                v-for="(item, index) in data_ls"
                :key="index">
                <td class="px-6 py-4">{{ index + 1 }}</td>
                <td class="px-6 py-4">
                  <a-tag color="green" v-if="item.total_net_price">5</a-tag>
                </td>
                <td class="px-6 py-4">
                  <a-tag color="red" v-if="!item.total_net_price"> 2</a-tag>
                </td>
                <td class="px-6 py-4">
                  <a @click.stop.prevent="activeKey = '1'">ดูรายละเอียด</a>
                </td>
              </tr>
            </tbody>
          </table>
        </div></a-tab-pane
      ><a-tab-pane key="1" tab="วัน">
        <div class="relative overflow-x-auto shadow-md">
          <table
            class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead
              class="text-xs"
              style="background-color: #1e88e5; color: white">
              <tr>
                <th scope="col" class="px-6 py-3">ลำดับ</th>
                <th scope="col" class="px-6 py-3">วัน เดือน ปี</th>
                <th scope="col" class="px-6 py-3">รายการ</th>
                <th scope="col" class="px-6 py-3">รายรับ</th>
                <th scope="col" class="px-6 py-3">รายจ่าย</th>
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
                  <a-tag color="green" v-if="item.total_net_price">{{
                    item.total_net_price
                  }}</a-tag>
                </td>
                <td class="px-6 py-4">
                  <a-tag color="red" v-if="!item.total_net_price">
                    {{ item.total_price }}</a-tag
                  >
                </td>
                <td class="px-6 py-4"></td>
              </tr>
            </tbody>
          </table></div
      ></a-tab-pane>
    </a-tabs>
  </v-container>
</template>
<script lang="ts">
import dayjs from "dayjs";
import { defineComponent } from "vue";
import { read_all_data } from "~~/services/pyapi";
import locale from "ant-design-vue/es/date-picker/locale/th_TH";
import buddhistEra from "dayjs/plugin/buddhistEra";
dayjs.extend(buddhistEra);
export default defineComponent({
  setup() {
    return {
      locale,
    };
  },
  data() {
    return {
      value1: "วัน",
      value2: "",
      data_ls: [] as any,
      data_ls2: [] as any,
      tours: [] as any,
      activeKey: "3",
    };
  },
  watch: {
    value2() {
      if (this.value1 == "วัน") {
        this.data_ls = this.data_ls2.filter(
          (x: any) =>
            x.date.split("/")[0] ==
            dayjs(this.value2).format("DD/MM/BBBB").split("/")[0]
        );
      } else if (this.value1 == "เดือน") {
        this.data_ls = this.data_ls2.filter(
          (x: any) =>
            x.date.split("/").slice(1, 2) ==
            dayjs(this.value2).format("DD/MM/BBBB").split("/").slice(1, 2)
        );
      } else if (this.value1 == "ปี") {
        this.data_ls = this.data_ls2.filter(
          (x: any) =>
            x.date.split("/")[2] ==
            dayjs(this.value2).format("DD/MM/BBBB").split("/")[2]
        );
      }
    },
  },
  async mounted() {
    const d1 = await read_all_data("estimates");
    const d2 = await read_all_data("quotations");
    this.tours = await read_all_data("tours");
    this.data_ls = d1.concat(d2);
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
      return sum;
    },
    sumTotalNetPrice() {
      let sum = 0;
      this.data_ls.forEach((x: any) => {
        if (x.total_net_price) {
          sum += x.total_net_price;
        }
      });
      return sum;
    },
    calculateTotalPrice() {
      return (this.sumTotalNetPrice() - this.sumTotalPrice()).toLocaleString(
        "th-TH",
        {
          minimumFractionDigits: 2,
        }
      );
    },
  },
});
</script>
