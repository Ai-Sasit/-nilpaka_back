<template>
  <div
    style="display: flex; background-color: rgb(225, 225, 241); z-index: -111"
  >
    <div class="page">
      <v-container>
        <v-row style="padding-right: 120px">
          <v-col style="padding: 0">
            <table
              class="border text-center text-sm font-light dark:border-neutral-500"
            >
              <tr class="border-b dark:border-neutral-500">
                <td
                  colspan="3"
                  class="whitespace-nowrap border-r dark:border-neutral-500 text-lg"
                >
                  <div>นายธนาธิป อภิรัตน์มนตรี</div>
                  <div style="font-size: 12px" class="text-blue-800">
                    MR.THANATHIP APHIRATMONTRI
                  </div>
                </td>
              </tr>
              <tr
                class="border-b border-black-200 bg-yellow-200 text-neutral-800"
              >
                <td
                  colspan="3"
                  class="whitespace-nowrap py-1 dark:border-neutral-500"
                >
                  เวียดนาม
                </td>
              </tr>
              <tr class="border-b dark:border-neutral-500">
                <td
                  colspan="1"
                  class="whitespace-nowrap border-r px-2 py-1 dark:border-neutral-500 text-pink-400 font-weight-bold"
                >
                  <div>NILPAKA TOUR</div>

                  <div style="font-size: 12px">
                    Jampahom999@hotmail.com | 098-1047644
                  </div>
                </td>
                <td
                  class="whitespace-nowrap py-3 text-red text-xl"
                  style="width: 80px"
                >
                  11
                </td>
              </tr>
            </table>
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
import { read_all_data, ArabicNumberToText } from "~~/services/pyapi";
export default defineComponent({
  setup() {
    dayjs.extend(buddhistEra);
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
