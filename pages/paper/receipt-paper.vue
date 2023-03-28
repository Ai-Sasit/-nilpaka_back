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
                        <b>ใบเสร็จรับเงิน</b>
                      </div>
                      <div>Receipt</div>
                      <div style="font-size: 10px">
                        ต้นฉบับ/Original (เอกสารออกเป็นชุด)
                      </div>
                    </td>
                  </tr>
                  <tr style="height: 20px; font-size: 12px">
                    <td style="width: 50%"><b>เลขที่ใบเสร็จรับเงิน: </b></td>
                    <td style="text-align: right">
                      {{ ob.receipt_no }}
                    </td>
                  </tr>
                  <tr style="height: 20px; font-size: 12px">
                    <td><b>วันที่รับชำระ: </b></td>
                    <td style="text-align: right">
                      {{ dayjs(ob.receipt_date).format("DD/MM/BBBB") }}
                    </td>
                  </tr>
                </v-table>
              </v-col>
            </v-row>
            <v-row style="font-size: 12px">
              <v-col cols="8"
                ><v-table>
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
                      <b>ที่อยู่: </b>
                    </td>
                    <td>
                      {{ quo.address }}
                    </td>
                  </tr>
                  <tr style="height: 20px">
                    <td><b>สาขา: </b></td>
                    <td>{{ ob.receipt_branch }}</td>
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
                    วันที่
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    เลขที่ใบกำกับภาษี
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    รายละเอียด
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    จำนวนเงิน
                  </td>
                </tr>
              </thead>
              <tbody style="font-weight: normal; font-size: 14px">
                <tr v-for="(item, index) in ob.tax_invoice" :key="index">
                  <td class="text-center" style="font-size: xx-small">
                    {{ index + 1 }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ item.date }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ item.tax_invoice_no }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ item.desc }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ parseFloat(item.total).toFixed(2) }}
                  </td>
                </tr>
              </tbody>
            </v-table>
          </v-col>
        </v-row>

        <v-row style="font-size: 12px; padding-left: 4px; margin: auto">
          <v-col style="padding: 0" cols="6">
            <v-table>
              <tr style="height: 20px">
                <td colspan="2" style="font-weight: bold">&nbsp;</td>
              </tr>
              <tr style="height: 30px">
                <td colspan="2" style="font-weight: bold">รวมทั้งสิ้น:</td>
                <td colspan="2">
                  {{
                    last_total.toLocaleString("th-TH", {
                      minimumFractionDigits: 2,
                    })
                  }}
                </td>
              </tr>
              <tr style="height: 30px">
                <td colspan="2" style="font-weight: bold">
                  รวมทั้งสิ้น ตัวอักษร:
                </td>
                <td colspan="2">{{ ArabicNumberToText(last_total) }}</td>
              </tr>
              <tr style="height: 30px">
                <td colspan="2" style="font-weight: bold">ชำระ:</td>
                <td colspan="2">{{ ob.receipt_pay_type }}</td>
              </tr>
              <tr style="height: 30px">
                <td colspan="2" style="font-weight: bold">
                  หักภาษี ณ ที่จ่าย :
                </td>
                <td colspan="2">{{ ob.receipt_tax }}</td>
              </tr>
              <tr style="height: 30px">
                <td colspan="2" style="font-weight: bold">
                  เลขที่เช็ค // วันที่เช็ค
                </td>
                <td colspan="2">
                  {{ ob.receipt_check_no }} //
                  {{ dayjs(ob.receipt_check_date).format("DD/MM/BBBB") }}
                </td>
              </tr>
            </v-table>
          </v-col>
        </v-row>

        <v-row style="font-size: 12px; padding-left: 4px; margin: auto">
          <v-col style="padding: 0">
            <v-table>
              <tr style="height: 30px">
                <td>
                  <b>หมายเหตุ: </b>
                  ใบเสร็จรับเงินฉบับนี้จะสมบูรณ์ต่อเมื่อได้รับเงินตามเช็คเรียบร้อยและต้องมีลายมือชื่อกรรมการ
                  ผู้มีอำนาจหรือผู้แทนที่ถูกต้องและผู้รับเงิน
                </td>
              </tr>
              <tr style="height: 20px">
                <td
                  colspan="2"
                  style="font-weight: bold; border-bottom: 1px solid black">
                  &nbsp;
                </td>
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
                <td>ผู้จัดทำ</td>
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
                <td>ผู้รับเงิน</td>
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
                <td>กรรมการผู้มีอำนาจหรือตัวแทน</td>
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
    justify="center"
    style="margin-top: -2rem; background-color: rgb(225, 225, 241)"
    class="hide-btn"
    ><v-col style="text-align: center"
      ><v-btn color="light-blue-accent-4" @click="print"
        >สั่งพิมพ์ หรือ บันทึกเป็น PDF</v-btn
      ></v-col
    ></v-row
  >
</template>
<script lang="ts">
import dayjs from "dayjs";
import { defineComponent } from "vue";
import buddhistEra from "dayjs/plugin/buddhistEra";
import { read_all_data, ArabicNumberToText } from "~~/services/pyapi";
export default defineComponent({
  setup() {
    dayjs.extend(buddhistEra);
    return {
      dayjs,
      ArabicNumberToText,
    };
  },
  async mounted() {
    const obj = JSON.parse(String(this.$route.query.data));
    this.ob = obj;

    this.last_total = obj.tax_invoice.reduce(
      (a: any, b: any) => Number(a) + Number(b.total),
      0
    );

    if (this.ob.receipt_tax == "3%") {
      this.last_total += this.last_total * 0.03;
    } else if (this.ob.receipt_tax == "5%") {
      this.last_total += this.last_total * 0.05;
    } else if (this.ob.receipt_tax == "1%") {
      this.last_total += this.last_total * 0.01;
    } else if (this.ob.receipt_tax == "4%") {
      this.last_total += this.last_total * 0.04;
    } else if (this.ob.receipt_tax == "7%") {
      this.last_total += this.last_total * 0.07;
    }
    const q = await read_all_data(
      `quotations?tour_id=${obj.tax_invoice[0].tour_id}`
    );
    this.quo = q[0];
    this.onLoad = true;
  },
  data() {
    return {
      onLoad: false,
      quo: {} as any,
      ob: {} as any,
      last_total: 0,
    };
  },
  methods: {
    print() {
      window.print();
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
</style>
