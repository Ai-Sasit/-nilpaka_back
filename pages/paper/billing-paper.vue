<template>
  <div
    style="display: flex; background-color: rgb(225, 225, 241); z-index: -111">
    <div class="page" v-if="onLoad">
      <v-container>
        <v-row style="margin: 2px">
          <v-col>
            <v-row
              ><img
                src="../../assets/imgs/ll-01.png"
                class="w-100px h-60px"
                alt="sridara Logo"
            /></v-row>
            <v-row style="border-bottom: 1px solid lightgray">
              <v-col cols="8"
                ><v-sheet style="text-align: left; font-size: 12px">
                  <div>ห้างหุ้นส่วนจำกัด นิลผกา</div>
                  <div>
                    49/175 ม.5 ต.หนองบัว อ.เมืองอุดรธานี จ.อุดรธานี 41000
                  </div>
                  <div>
                    เลขประจำตัวผู้เสียภาษี: 413558001156 | โทร.098-1047644
                  </div>

                  <div><a>www.facebook.com/Jampahomtour</a></div>
                </v-sheet></v-col
              >
              <v-col>
                <v-table>
                  <tr style="text-align: center">
                    <td colspan="2">
                      <div style="font-size: 14px">
                        <b>ใบวางบิล/ใบแจ้งหนี้</b>
                      </div>
                      <div>Billing Note/Invoice</div>
                    </td>
                  </tr>
                  <tr style="height: 20px; font-size: 12px">
                    <td style="width: 50%"><b>เลขที่:</b></td>
                    <td style="text-align: right">{{ bill.no }}</td>
                  </tr>
                  <tr style="height: 20px; font-size: 12px">
                    <td><b>วันที่:</b></td>
                    <td style="text-align: right">{{ bill.date }}</td>
                  </tr>
                </v-table>
              </v-col>
            </v-row>
            <v-row style="font-size: 12px">
              <v-col cols="8"
                ><v-table style="border-right: 1px solid lightgray">
                  <tr style="height: 20px">
                    <td style="width: 20%">
                      <b>ชื่อลูกค้า: </b>
                    </td>
                    <td style="text-align: left">
                      {{ quo.customer_name }}
                    </td>
                  </tr>
                  <tr style="height: 20px">
                    <td>
                      <b>ชื่อผู้ติดต่อ: </b>
                    </td>
                    <td>{{ quo.contact_name }}</td>
                  </tr>
                  <tr style="height: 20px">
                    <td><b>ที่อยู่: </b></td>
                    <td>{{ quo.address }}</td>
                  </tr>
                  <tr style="height: 20px">
                    <td>
                      <b>ID TAX: </b>
                    </td>
                    <td>{{ quo.tax_id }}</td>
                  </tr>
                  <tr style="height: 20px">
                    <td>
                      <b>FAX: </b>
                    </td>
                    <td>{{ bill.fax }}</td>
                  </tr>
                  <tr style="height: 20px">
                    <td>
                      <b>โทร: </b>
                    </td>
                    <td>{{ quo.customer_tel }}</td>
                  </tr>
                </v-table></v-col
              >
              <v-col
                ><v-table style="font-size: 12px">
                  <tr style="height: 20px">
                    <td style="width: 50%"><b>รหัสลูกค้า:</b></td>
                    <td style="text-align: right">{{ quo.customer_code }}</td>
                  </tr>
                  <tr style="height: 20px">
                    <td><b>ผู้เสนอขาย:</b></td>
                    <td style="text-align: right">{{ quo.sales_person }}</td>
                  </tr>
                  <tr style="height: 20px">
                    <td><b>ฝ่าย:</b></td>
                    <td style="text-align: right">{{ quo.sale_department }}</td>
                  </tr>
                  <tr style="height: 20px">
                    <td><b>ยืนยันราคาวันที่:</b></td>
                    <td style="text-align: right">
                      {{ quo.confirm_price_within }}
                    </td>
                  </tr>
                </v-table></v-col
              >
            </v-row>
          </v-col>
        </v-row>

        <v-row
          style="padding: 1px; margin: auto; border-bottom: 1px solid black">
          <v-col style="padding: 1px; height: 400px">
            <v-table density="compact" height="auto">
              <thead style="font-weight: bold; font-size: 14px">
                <tr
                  style="
                    border-top: 1px solid black;
                    border-bottom: 1px solid black;
                  ">
                  <td class="text-center" style="font-size: xx-small">ลำดับ</td>
                  <td class="text-center" style="font-size: xx-small">
                    รหัสสินค้า
                  </td>
                  <td class="text-left" style="font-size: xx-small">
                    รายการสินค้า
                  </td>
                  <td class="text-center" style="font-size: xx-small">จำนวน</td>
                  <td class="text-center" style="font-size: xx-small">
                    ราคาต่อหน่วย
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    ส่วนลด
                  </td>
                  <td class="text-center" style="font-size: xx-small">ภาษี</td>
                  <td class="text-center" style="font-size: xx-small">
                    จำนวนเงิน
                  </td>
                </tr>
              </thead>
              <tbody style="font-weight: normal; font-size: 14px">
                <tr v-for="(item, index) in prod" :key="index">
                  <td class="text-center" style="font-size: xx-small">
                    {{ index + 1 }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ item.code }}
                  </td>
                  <td class="text-left" style="font-size: xx-small">
                    {{ item.name }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ item.qty }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ item.price_per_unit }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ item.discount }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ item.tax }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ item.amount }}
                  </td>
                </tr>
              </tbody>
            </v-table>
          </v-col>
        </v-row>

        <v-row
          style="
            font-size: 12px;
            padding-left: 4px;
            padding-right: 4px;
            margin: auto;
            border-bottom: 1px solid black;
          ">
          <v-col style="padding: 0" cols="9"
            ><v-table>
              <tr>
                <td colspan="6">&nbsp;</td>
              </tr>
              <tr>
                <td colspan="6"><b>หมายเหตุ:</b></td>
              </tr>
              <tr>
                <td colspan="6">
                  1.สินค้าตามรายการข้างต้นแม้จะได้รับมอบแก่ผู้ซื้อแล้วก็ยังคงเป็นสินทรัพย์ของผู้ขายจนกว่าผู้ซื้อจะชำระเงินเรียบร้อย
                </td>
              </tr>
              <tr>
                <td colspan="6">
                  2.หากชำระเงินด้วยเช็คโปรดขีดคร่อมในนาม
                  “บริษัทศรีดาราทัวร์จำกัด” และขีดฆ่าหรือผู้ถือออก
                </td>
              </tr>
              <tr>
                <td colspan="6">
                  3.การชำระเงินด้วยเช็คจะสมบูรณ์เมื่อ
                  บริษัทฯได้รับเงินตามเช็คเรียบร้อย
                </td>
              </tr>
              <tr>
                <td colspan="2">
                  <b>ตัวอักษร: </b>
                </td>
                <td colspan="4">
                  <b> {{ ArabicNumberToText(Number(quo.total_net_price)) }}</b>
                </td>
              </tr>
              <tr>
                <td colspan="6">&nbsp;</td>
              </tr>
            </v-table>
          </v-col>
          <v-col style="padding: 0" cols="3"
            ><v-table style="padding-left: 20px; padding-right: 10px">
              <tr>
                <td>&nbsp;</td>
                <td>&nbsp;</td>
              </tr>
              <tr>
                <td>รวมเงิน</td>
                <td style="text-align: right">{{ quo.total_price }} บาท</td>
              </tr>
              <tr>
                <td>มัดจำ 100%</td>
                <td style="text-align: right">{{ quo.earnest_money }} บาท</td>
              </tr>
              <tr>
                <td>มูลค่าคิดภาษี</td>
                <td style="text-align: right">{{ quo.net_price }} บาท</td>
              </tr>
              <tr>
                <td>ภาษีมูลค่าเพิ่ม</td>
                <td style="text-align: right">{{ quo.vat }} บาท</td>
              </tr>
              <tr>
                <td><b>ยอดสุทธิ</b></td>
                <td style="text-align: right">{{ quo.total_net_price }} บาท</td>
              </tr>
              <tr>
                <td>&nbsp;</td>
                <td>&nbsp;</td>
              </tr>
            </v-table>
          </v-col>
        </v-row>

        <v-row style="margin: auto; font-size: 12px">
          <v-col>
            <v-table style="text-align: center">
              <tr style="height: 110px">
                <td>&nbsp;</td>
              </tr>
              <tr>
                <td>ผู้รับสินค้า</td>
              </tr>
              <tr>
                <td>วันที่</td>
              </tr>
              <tr style="border-bottom: 1px solid black">
                <td>&nbsp;</td>
              </tr>
            </v-table>
          </v-col>
          <v-col>
            <v-table style="text-align: center">
              <tr style="height: 110px">
                <td>&nbsp;</td>
              </tr>
              <tr>
                <td>ผู้ส่งสินค้า</td>
              </tr>
              <tr>
                <td>วันที่</td>
              </tr>
              <tr style="border-bottom: 1px solid black">
                <td>&nbsp;</td>
              </tr>
            </v-table>
          </v-col>
          <v-col>
            <v-table style="text-align: center">
              <tr style="height: 110px">
                <td>&nbsp;</td>
              </tr>
              <tr>
                <td>ผู้ประสานงาน</td>
              </tr>
              <tr>
                <td>วันที่</td>
              </tr>
              <tr style="border-bottom: 1px solid black">
                <td>&nbsp;</td>
              </tr>
            </v-table>
          </v-col>
          <v-col>
            <v-table style="text-align: center">
              <tr style="height: 110px">
                <td>&nbsp;</td>
              </tr>
              <tr>
                <td>ผู้อนุมัติ</td>
              </tr>
              <tr>
                <td>วันที่</td>
              </tr>
              <tr style="border-bottom: 1px solid black">
                <td>&nbsp;</td>
              </tr>
            </v-table>
          </v-col>
        </v-row>
      </v-container>
    </div>
  </div>
  <v-row
    v-if="onLoad"
    justify="center"
    style="margin-top: -2rem; background-color: rgb(225, 225, 241)"
    class="hide-btn"
    ><v-col style="text-align: right">
      <v-btn color="yellow-darken-4" @click="dialog = true"
        >แก้ไขเอกสารใบแจ้งหนี้</v-btn
      ></v-col
    ><v-col style="text-align: left">
      <v-btn color="light-blue-accent-4" @click="print"
        >สั่งพิมพ์ หรือ บันทึกเป็น PDF</v-btn
      ></v-col
    ></v-row
  >

  <a-modal v-model:visible="dialog" title="ฟอร์มสร้างแก้ไขแจ้งหนี้/ใบวางบิล">
    <template #footer>
      <a-button key="back" @click="dialog = false">ยกเลิก</a-button>
      <a-button
        key="submit"
        type="primary"
        :loading="loadGenBill"
        @click="updateBilling"
        >แก้ไข</a-button
      >
    </template>
    <v-row>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >เลขที่</label
        >
        <input
          type="text"
          id="base-input"
          v-model="billing.billing_note_no"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" />
      </v-col>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >วันที่</label
        >
        <a-date-picker
          :locale="locale"
          style="z-index: 999"
          v-model:value="billing.billing_note_date"
          class="date-picker"
          format="DD/MM/YYYY" />
      </v-col>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >หมายเลขแฟกซ์</label
        >
        <input
          type="text"
          id="base-input"
          v-model="billing.billing_note_fax"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" />
      </v-col>
    </v-row>
  </a-modal>
</template>
<script lang="ts">
import { defineComponent } from "vue";
import {
  read_all_data,
  ArabicNumberToText,
  update_data,
  read_one_data,
} from "~~/services/pyapi";
import locale from "ant-design-vue/es/date-picker/locale/th_TH";
import dayjs from "dayjs";
import buddhistEra from "dayjs/plugin/buddhistEra";
dayjs.extend(buddhistEra);
const key = "updated";
export default defineComponent({
  setup() {
    return {
      ArabicNumberToText,
      locale,
    };
  },
  data() {
    return {
      quo_id: "",
      tour_id: "",
      quo: {} as any,
      bill: {} as any,
      prod: [] as any,
      onLoad: false,
      dialog: false,
      billing: {
        billing_note_no: "",
        billing_note_date: "" as any,
        billing_note_fax: "",
      },
      loadGenBill: false,
    };
  },
  async mounted() {
    this.quo_id = String(this.$route.query.qid);
    this.$message.loading({
      content: "กำลังโหลดข้อมูลใบแจ้งหนี้ และสร้างเป็นเอกสาร",
      key,
    });
    this.quo = await read_one_data(`quotation`, this.quo_id);
    this.tour_id = this.quo.tour_id;
    let b = await read_all_data(`billings?quo_id=${this.quo_id}`);
    this.bill = b[0];
    this.billing.billing_note_no = this.bill.no;
    this.billing.billing_note_date = dayjs();
    this.billing.billing_note_fax = this.bill.fax;
    this.prod = await read_all_data(`products?qid=${this.quo.no}`);
    this.onLoad = true;
    this.$message.success({
      content: "โหลดข้อมูลเรียบร้อยแล้ว",
      key,
      duration: 2,
    });
  },
  methods: {
    print() {
      window.print();
    },
    validateBillingForm() {
      if (this.billing.billing_note_no == "") {
        this.$message.error("กรุณากรอกเลขที่ใบแจ้งหนี้");
        return false;
      }
      if (this.billing.billing_note_date == "") {
        this.$message.error("กรุณากรอกวันที่ใบแจ้งหนี้");
        return false;
      }
      if (this.billing.billing_note_fax == "") {
        this.$message.error("กรุณากรอกหมายเลขแฟกซ์");
        return false;
      }
      return true;
    },
    updateBilling() {
      if (this.validateBillingForm()) {
        this.loadGenBill = true;
        const payload = {
          tour_id: this.tour_id,
          quo_id: this.quo_id,
          no: this.billing.billing_note_no,
          date: dayjs(this.billing.billing_note_date).format("DD/MM/BBBB"),
          fax: this.billing.billing_note_fax,
        };
        update_data("billing", this.bill.id, payload).then((res) => {
          this.$message.success({
            content: "บันทึกข้อมูลเรียบร้อยแล้ว",
            key,
            duration: 2,
          });
          this.dialog = false;
          window.location.reload();
        });
      }
    },
  },
});
</script>

<style scope>
@page {
  size: A4;
  margin: 0;
}

* {
  box-sizing: border-box;
}
.page {
  width: 210mm;
  min-height: 297mm;
  padding: 2mm;
  margin: 10mm auto;
  border: 1px #d3d3d3 solid;
  border-radius: 5px;
  background: white;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
}

@media print {
  header {
    display: none;
  }
  .hide-btn {
    display: none;
  }
  html,
  body {
    width: 210mm;
    height: 297mm;
  }
  .page {
    margin: 0;
    border: initial;
    border-radius: initial;
    width: initial;
    min-height: initial;
    box-shadow: initial;
    background: initial;
    page-break-after: always;
  }
}
.date-picker {
  height: 4.7vmin;
  background-color: #f9fafb;
  border-radius: 0.4rem;
  width: 100%;
}
</style>
