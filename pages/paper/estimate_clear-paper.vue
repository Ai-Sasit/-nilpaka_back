<template>
  <div
    style="display: flex; background-color: rgb(225, 225, 241); z-index: -111">
    <div class="page">
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
                      <b style="font-size: 14px"
                        >ใบเคลียร์ประมาณการเงินสดย่อย</b
                      >
                      <div>Estimate Clearing</div>
                    </td>
                  </tr>
                  <tr style="height: 20px; font-size: 12px">
                    <td style="width: 50%"><b>เลขที่:</b></td>
                    <td style="text-align: right">{{ ob.no }}</td>
                  </tr>
                  <tr style="height: 20px; font-size: 12px">
                    <td><b>วันที่:</b></td>
                    <td style="text-align: right">{{ ob.date }}</td>
                  </tr>
                </v-table>
              </v-col>
            </v-row>
            <v-row style="font-size: 12px">
              <v-col cols="8"
                ><v-table style="border-right: 1px solid lightgray">
                  <tr style="height: 20px">
                    <td style="width: 50%">
                      <b>ผู้ขอเคลียร์: </b>
                    </td>
                    <td style="text-align: left">
                      {{ ob.person_clear }}
                    </td>
                  </tr>
                  <tr style="height: 20px">
                    <td>
                      <b>อ้างอิงถึงเลขที่ใบขอเคลียร์เงินทดลองจ่าย: </b>
                    </td>
                    <td>
                      {{ ob.pefer_no_example_pay }}
                    </td>
                  </tr>
                </v-table></v-col
              >
              <v-col
                ><v-table style="font-size: 12px">
                  <tr style="height: 20px">
                    <td style="width: 50%"><b>ฝ่าย/แผนก:</b></td>
                    <td style="text-align: right">
                      {{ ob.department }}
                    </td>
                  </tr>
                  <tr style="height: 20px">
                    <td><b>โครงการ:</b></td>
                    <td style="text-align: right">
                      {{ ob.tour_name }}
                    </td>
                  </tr>
                </v-table>
              </v-col>
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
                    วันที่ใบเสร็จ
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    รายละเอียดค่าใช้จ่าย
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    เลขที่ใบเสร็จ
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    เลขประจำตัวผู้เสียภาษี
                  </td>
                  <td class="text-right" style="font-size: xx-small">
                    ยอดสุทธิ
                  </td>
                </tr>
              </thead>
              <tbody style="font-weight: normal; font-size: 14px">
                <tr
                  style="border-bottom: 1px solid black"
                  :key="index"
                  v-for="(items, index) in desc_ls">
                  <td class="text-center" style="font-size: xx-small">
                    {{ index + 1 }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ items.clear_date }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ items.clear_desc }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ items.clear_no }}
                  </td>
                  <td class="text-center" style="font-size: xx-small">
                    {{ items.clear_tax_id }}
                  </td>
                  <td class="text-right" style="font-size: xx-small">
                    {{
                      Number(items.clear_total).toLocaleString("th-TH", {
                        minimumFractionDigits: 2,
                      })
                    }}
                  </td>
                </tr>
              </tbody>
            </v-table>
          </v-col>
        </v-row>

        <v-row style="font-size: 12px; padding-left: 4px; margin: auto">
          <v-col style="padding: 0">
            <v-table>
              <tr style="height: 20px">
                <td colspan="5" style="font-weight: bold">&nbsp;</td>
              </tr>
              <tr style="height: 30px">
                <td align="right" colspan="2" style="font-weight: bold">
                  รวมสุทธิ :
                </td>
                <td align="center" colspan="2">
                  {{
                    Number(ob.total).toLocaleString("th-TH", {
                      minimumFractionDigits: 2,
                    })
                  }}
                  .-
                </td>
              </tr>
              <tr style="height: 30px">
                <td align="right" colspan="2" style="font-weight: bold">
                  ยอกเงินประมาณการ :
                </td>
                <td align="center" colspan="2">
                  {{
                    Number(ob.total_predict).toLocaleString("th-TH", {
                      minimumFractionDigits: 2,
                    })
                  }}
                  .-
                </td>
              </tr>
              <tr style="height: 30px">
                <td align="right" colspan="2" style="font-weight: bold">
                  ยอดเงินส่งคืน :
                </td>
                <td align="center" colspan="2">
                  {{
                    ob.total_predict - ob.total < 0
                      ? (0).toLocaleString("th-TH", {
                          minimumFractionDigits: 2,
                        })
                      : Number(ob.total_predict - ob.total).toLocaleString(
                          "th-TH",
                          {
                            minimumFractionDigits: 2,
                          }
                        )
                  }}
                  .-
                </td>
              </tr>
              <tr style="height: 30px">
                <td align="right" colspan="2" style="font-weight: bold">
                  ยอดเงินเบิกเพิ่ม :
                </td>
                <td align="center" colspan="2">
                  {{
                    ob.total - ob.total_predict < 0
                      ? (0).toLocaleString("th-TH", {
                          minimumFractionDigits: 2,
                        })
                      : Number(ob.total - ob.total_predict).toLocaleString(
                          "th-TH",
                          {
                            minimumFractionDigits: 2,
                          }
                        )
                  }}
                  .-
                </td>
              </tr>
            </v-table>
          </v-col>
        </v-row>

        <v-row style="font-size: 12px; padding-left: 4px; margin: auto">
          <v-col style="padding: 0">
            <v-table>
              <tr style="height: 30px">
                <td colspan="2" style="font-weight: bold">
                  รายละเอียดโครงการ/กิจกรรม : {{ ob.tour_desc }}
                </td>
              </tr>
              <tr style="height: 30px">
                <td colspan="2" style="font-weight: bold">
                  ผู้อนุมัติ (แบบอิเล็กทอรนิกส์)&nbsp;&nbsp;&nbsp;:
                  {{ ob.approve_user }}
                </td>
              </tr>
              <tr style="height: 20px">
                <td
                  colspan="3"
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
                <td>(นางสาวสุริพร รัตนชัย)</td>
              </tr>
              <tr>
                <td>ผู้ขอเคียร์</td>
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
                <td>(………………………………………………)</td>
              </tr>
              <tr>
                <td>ผู้อนุมัติ / ตรวจสอบ</td>
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
                <td>(………………………………………………)</td>
              </tr>
              <tr>
                <td>ผู้รับเคียร์เงิน</td>
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
                <td>(………………………………………………)</td>
              </tr>
              <tr>
                <td>ผู้อำนวยการฝ่ายการเงิน</td>
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
      ><v-btn color="light-blue-accent-4"
        >สั่งพิมพ์ หรือ บันทึกเป็น PDF</v-btn
      ></v-col
    ></v-row
  >
</template>
<script lang="ts">
import dayjs from "dayjs";
import { defineComponent } from "vue";
import buddhistEra from "dayjs/plugin/buddhistEra";
import { ArabicNumberToText } from "~~/services/pyapi";
export default defineComponent({
  setup() {
    dayjs.extend(buddhistEra);
    return {
      dayjs,
      ArabicNumberToText,
    };
  },
  mounted() {
    const obj = JSON.parse(String(this.$route.query.data));
    console.log(obj);
    this.ob = obj;
    const temp = obj.desc_ls.map((e: any) => e.clearance);
    this.desc_ls = [].concat(...temp);
  },
  data() {
    return {
      onLoad: false,
      quo: {} as any,
      ob: {} as any,
      desc_ls: [] as any,
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
