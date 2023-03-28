<template>
  <section
    class="bg-gray-50 dark:bg-gray-900 p-3 sm:p-5"
    style="margin-top: 1rem">
    <v-card color="light-blue-darken-1" v-if="loading">
      <v-card-text>
        กำลังโหลดข้อมูล กรุณารอ...
        <v-progress-circular
          style="margin-left: 1rem; margin-bottom: 0.3rem"
          indeterminate
          color="white"></v-progress-circular>
      </v-card-text>
    </v-card>
    <div v-else>
      <v-tooltip location="left">
        <template v-slot:activator="{ props }">
          <v-btn
            icon
            @click="$router.push(`/edittour/${tour_data.id}`)"
            style="position: absolute; right: 0.2rem; top: 4.5rem; z-index: 999"
            color="blue-accent-3"
            v-bind="props">
            <v-icon> mdi-text-box-edit </v-icon>
          </v-btn>
        </template>
        <span>แก้ไขข้อมูลทัวร์ หรือ เพิ่มโรงแรม</span>
      </v-tooltip>
      <v-tooltip location="left">
        <template v-slot:activator="{ props }">
          <v-btn
            icon
            @click="tour_comment_dialog = true"
            style="position: absolute; right: 0.2rem; top: 8rem; z-index: 999"
            color="deep-purple-accent-4"
            v-bind="props">
            <v-icon> mdi-comment-text </v-icon>
          </v-btn>
        </template>
        <span>อัพเดท คอมเมนต์</span>
      </v-tooltip>
      <v-tooltip location="left">
        <template v-slot:activator="{ props }">
          <v-btn
            icon
            :href="`/paper/sticker?tid=${tour_data.id}&name=${tour_data.program_name}`"
            style="
              position: absolute;
              right: 0.2rem;
              top: 11.5rem;
              z-index: 999;
            "
            color="purple-accent-4"
            v-bind="props">
            <v-icon> mdi-sticker-text </v-icon>
          </v-btn>
        </template>
        <span>พิมพ์รายชื่อ Tag</span>
      </v-tooltip>
      <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
        <table
          v-if="tour_data"
          class="w-full text-sm text-left text-gray-500 dark:text-gray-400"
          style="background-color: #e1f5fe">
          <tr>
            <td colspan="5" class="pb-1 pt-2 px-3">
              <h3>ชื่อทริปทัวร์ : {{ tour_data.name }}</h3>
            </td>
            <td colspan="2" class="pb-1 pt-2 px-3">
              ชื่อไกด์ :
              {{ tour_data.guided_tour.map((v) => v.name).join(", ") }}
            </td>
            <td colspan="4" class="pb-1 pt-2 px-3">
              เบอร์โทร :
              {{ tour_data.guided_tour.map((v) => v.tel).join(", ") }}
            </td>
          </tr>
          <tr>
            <td colspan="3" class="py-1 pt-2 px-3">
              <h3>โปรแกรมทัวร์ : {{ tour_data.program_name }}</h3>
            </td>
            <td colspan="2" class="py-1 px-3">
              จำนวน : {{ tour_data.amount_of_days }} วัน
              {{ tour_data.amount_of_nights }} คืน
            </td>
            <td colspan="2" class="py-1 px-3">
              วันที่เดือนปี {{ tour_data.date_go }} ถึง
              {{ tour_data.date_go }}
            </td>
            <td colspan="4" class="py-1 px-3">
              จำนวนลูกทัวร์ : {{ members_ls.length }}
            </td>
          </tr>

          <tr
            style="background-color: #fffde7"
            v-for="(item, index) in hotels_ls"
            :key="index">
            <td colspan="3" class="py-1 px-3">
              <h3>ชื่อโรงแรม : {{ item.name }}</h3>
            </td>
            <td colspan="2" class="py-1 px-3">
              จำนวนห้องพัก : {{ item.amount_of_rooms }}
            </td>
            <td colspan="2" class="py-1 px-3">
              วันที่เช็คอินน์ : {{ item.check_in }}
            </td>
            <td colspan="4" class="py-1 px-3">
              วันที่เช็คเอ้าท์ : {{ item.check_out }}
            </td>
          </tr>

          <tr>
            <td colspan="5" class="py-1 px-3">
              <h3>
                เที่ยวบินหรือพาหนะอื่น ๆ ขาไป :
                {{ tour_data.vehicle_in }}
              </h3>
            </td>
            <td colspan="2" class="py-1 px-3">
              <h3>
                เที่ยวบินหรือพาหนะอื่น ๆ ขากลับ :
                {{ tour_data.vehicle_out }}
              </h3>
            </td>
            <td colspan="5" class="py-1 px-3">
              <h3>หมายเหตุ : {{ tour_data.comments }}</h3>
            </td>
          </tr>
        </table>
        <table
          class="w-full text-sm text-left text-gray-500 dark:text-gray-400"
          :style="
            members_ls.length >= 10
              ? { 'max-height': '400px', 'margin-bottom': '2.5rem' }
              : { 'max-height': '400px' }
          ">
          <thead
            class="text-xs text-gray-700 uppercase bg-gray-50 dark:text-gray-400"
            style="background-color: #0277bd; color: white">
            <tr>
              <th scope="col" class="px-6 py-3">ลำดับ</th>
              <th scope="col" class="px-6 py-3">ชื่อ-นามสกุลภาษาไทย</th>
              <th scope="col" class="px-6 py-3">หมายเลขบัตรประชาชน</th>
              <th scope="col" class="px-6 py-3">ชื่อ-นามสกุลภาษาอังกฤษ</th>
              <th scope="col" class="px-6 py-3">หมายเลขพาสปอร์ต</th>
              <th scope="col" class="px-6 py-3">สัญชาต</th>
              <th scope="col" class="px-6 py-3">เพศ</th>
              <th scope="col" class="px-6 py-3">ประเภทเตียง</th>
              <th scope="col" class="px-6 py-3">เพิ่มเติม</th>
            </tr>
          </thead>
          <tbody v-if="!members_ls.length">
            <tr>
              <td class="px-6 py-4" colspan="12" style="text-align: center">
                ไม่มีข้อมูล
              </td>
            </tr>
          </tbody>
          <tbody v-if="members_ls.length">
            <tr
              class="bg-white border-b dark:bg-gray-800 dark:border-gray-700"
              v-for="(item, index) in members_ls"
              :key="index">
              <td class="px-6 py-4" style="font-size: 13px">{{ index + 1 }}</td>
              <td class="px-6 py-4" style="font-size: 13px">
                {{ item.thai_name }}
              </td>
              <td class="px-6 py-4" style="font-size: 13px">
                {{ item.national_id }}
              </td>
              <td class="px-6 py-4" style="font-size: 13px">
                {{ item.eng_name }}
              </td>
              <td class="px-6 py-4" style="font-size: 13px">
                {{ item.passport_no }}
              </td>
              <td class="px-6 py-4" style="font-size: 13px">
                {{ item.nationality }}
              </td>
              <td class="px-6 py-4" style="font-size: 13px">
                {{ item.gender }}
              </td>
              <td class="px-6 py-4" style="font-size: 13px">
                {{ item.bed_type }}
              </td>
              <td class="px-6 py-4" style="font-size: 13px">
                <a @click.stop.prevent="openMemberDetailDialog(item)"
                  >รายละเอียดเพิ่มเติม</a
                >
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </section>

  <a-modal
    v-model:visible="member_detail_dialog"
    :title="`รายละเอียดลูกทัวร์ | ${selected_member.thai_name}`">
    <template #footer>
      <a-row justify="space-between">
        <a-col>
          <a-input-group compact>
            <a-input
              v-model:value="member_comment"
              placeholder="ข้อความ"
              style="width: 30vmin; text-align: left" />
            <a-button type="primary" @click="handleUpdateMemberComment"
              >คอมเมนต์</a-button
            >
          </a-input-group>
        </a-col>
        <a-col>
          <a-button
            key="submit"
            type="danger"
            @click="member_detail_dialog = false"
            >ปิด</a-button
          ></a-col
        >
      </a-row>
    </template>
    <p>วันที่ออกพาสปอร์ต: {{ selected_member.passport_issue }}</p>
    <p>วันหมดอายุพาสปอร์ต: {{ selected_member.passport_exp }}</p>
    <p>วันเดือนปีเกิด: {{ selected_member.date_of_birth }}</p>
    <p>ตรวจลงตราเลขที: {{ selected_member.stamp_no }}</p>
    <p>หมายเหตุ: {{ selected_member.vip_u }}</p>
  </a-modal>

  <v-row
    justify="space-between"
    v-if="!isMobile"
    style="
      background-color: #eceff1;
      box-shadow: rgba(67, 71, 85, 0.27) 0px 0px 0.25em,
        rgba(90, 125, 188, 0.05) 0px 0.25em 1em;
      width: 100%;
      margin: 0;
      position: fixed;
      bottom: 0;
    ">
    <v-col cols="2">
      <v-row>
        <v-col style="text-align: left"
          ><v-btn
            variant="tonal"
            color="red-accent-4"
            @click="handleDelete(tour_data.name)"
            >ลบทัวร์</v-btn
          ></v-col
        ></v-row
      ></v-col
    >
    <v-col style="display: flex; align-items: center">
      <v-row justify="end">
        <v-btn
          style="margin-right: 1rem"
          @click="resturaunt_dialog = true"
          variant="tonal"
          color="green-darken-4"
          >จัดการร้านอาหาร</v-btn
        >
        <v-btn
          style="margin-right: 1rem"
          @click="attraction_dialog = true"
          variant="tonal"
          color="green-darken-4"
          >จัดการสถานที่เที่ยว</v-btn
        >
        <v-btn
          style="margin-right: 1rem"
          @click="direction_dialog = true"
          variant="tonal"
          color="green-darken-4"
          >จัดการเส้นทาง</v-btn
        >
        <v-btn
          style="margin-right: 1rem"
          variant="tonal"
          @click="hotel_comment_dialog = true"
          color="green-darken-4"
          >คอมเมนต์โรงแรม</v-btn
        >
        <v-btn
          style="margin-right: 1rem"
          @click="list_cash = true"
          variant="tonal"
          color="light-blue-darken-4"
          >จัดการใบเบิกค่าใช้จ่าย</v-btn
        >
        <v-btn
          style="margin-right: 1rem"
          variant="tonal"
          color="light-blue-darken-4"
          @click="list_quo = true"
          >จัดการใบเสนอราคา</v-btn
        >
        <v-btn
          style="margin-right: 1rem"
          variant="tonal"
          @click="$router.push(`/addusertour/${tour_id}`)"
          color="yellow-darken-4"
          >จัดการข้อมูลลูกทัวร์</v-btn
        >
      </v-row></v-col
    >
  </v-row>

  <button
    v-else
    title="Contact Sale"
    @click="tools = true"
    style="box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px"
    class="fixed z-90 bottom-5 right-5 bg-blue-600 w-15 h-15 rounded-full drop-shadow-lg flex justify-center items-center text-white text-4xl hover:bg-blue-700 hover:drop-shadow-2xl hover:animate-bounce duration-300">
    <Icon name="material-symbols:keyboard-double-arrow-up"></Icon>
  </button>

  <a-modal v-model:visible="list_quo" width="65rem" title="รายการใบเสนอราคา">
    <template #footer>
      <a-button type="primary" @click="onCreateQuotation"
        >สร้างใบเสนอราคา</a-button
      >
      <a-button type="danger" @click="list_quo = false">ปิด</a-button>
    </template>

    <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
      <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
        <thead
          class="text-xs uppercase bg-green-800 dark:bg-gray-700"
          style="color: white">
          <tr>
            <th scope="col" class="px-6 py-3">ชื่อลูกค้า</th>
            <th scope="col" class="px-6 py-3">ชื่อผู้เสนอขาย</th>
            <th scope="col" class="px-6 py-3">วันที่สร้างล่าสุด</th>
            <th scope="col" class="px-6 py-3">ราคาเสนอ</th>
            <th scope="col" class="px-6 py-3 text-right">ฟังก์ชั่น</th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600"
            v-for="(item, index) in quo_ls"
            :key="index">
            <th
              scope="row"
              class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
              {{ item.customer_name }}
            </th>
            <td class="px-6 py-4">{{ item.sales_person }}</td>
            <td class="px-6 py-4">{{ item.date }}</td>
            <td class="px-6 py-4">
              {{ formatter.format(item.total_net_price) }}
            </td>
            <td class="px-6 py-4 text-right">
              <a
                :href="`/paper/quotation-paper?tid=${tour_id}&cid=${item.no}`"
                class="font-medium text-yellow-400 dark:text-yellow-400 hover:underline mr-5"
                >ดูใบเสนอราคา</a
              >
              <a-dropdown>
                <a class="ant-dropdown-link text-blue-600 mr-5" @click.prevent>
                  ใบอื่นๆ
                </a>
                <template #overlay>
                  <a-menu>
                    <a-menu-item v-if="!item.haveBilling">
                      <a @click.stop.prevent="onOpenBillDialog(item)"
                        >สร้างใบแจ้งหนี้</a
                      >
                    </a-menu-item>
                    <a-menu-item v-else>
                      <a :href="`/paper/billing-paper?qid=${item.id}`"
                        >ดูใบแจ้งหนี้</a
                      >
                    </a-menu-item>
                    <a-menu-item v-if="!item.haveTax">
                      <a @click.stop.prevent="onOpenTaxDialog(item)"
                        >สร้างใบกำกับภาษี</a
                      >
                    </a-menu-item>
                    <a-menu-item v-else>
                      <a
                        :href="`/paper/tax-paper?qid=${item.id}&cid=${item.customer_code}`"
                        >ดูใบกำกับภาษี</a
                      >
                    </a-menu-item>
                  </a-menu>
                </template>
              </a-dropdown>
              <a-popconfirm
                title="คุณแน่ใจ?"
                ok-text="ยืนยัน"
                cancel-text="ยกเลิก"
                @confirm="onDeleteQuotation(item.id)"
                @cancel="cancel">
                <a
                  href="#"
                  class="font-medium text-red-600 dark:text-red-500 hover:underline"
                  >ลบ</a
                ></a-popconfirm
              >
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </a-modal>

  <a-modal
    v-model:visible="list_cash"
    width="65rem"
    title="รายการใบเบิกค่าใช้จ่าย">
    <template #footer>
      <a-button
        type="primary"
        @click="$router.push(`/withdraw/add_estimate?tour_id=${tour_id}`)"
        >สร้างใบเบิกค่าใช้จ่าย</a-button
      >
      <a-button type="danger" @click="list_cash = false">ปิด</a-button>
    </template>

    <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
      <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
        <thead
          class="text-xs uppercase bg-green-800 dark:bg-gray-700"
          style="color: white">
          <tr>
            <th scope="col" class="px-6 py-3">ชื่อผู้เบิก</th>
            <th scope="col" class="px-6 py-3">วันที่เบิก</th>
            <th scope="col" class="px-6 py-3">วัตถุประสงค์</th>
            <th scope="col" class="px-6 py-3">ยอดสุทธิ</th>
            <th scope="col" class="px-6 py-3 text-right">ฟังก์ชั่น</th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600"
            v-for="(item, index) in estimate_ls"
            :key="index">
            <th
              scope="row"
              class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
              {{ item.withdrawer_name }}
            </th>
            <td class="px-6 py-4">{{ item.date }}</td>
            <td class="px-6 py-4">{{ item.objective }}</td>
            <td class="px-6 py-4">
              {{ formatter.format(item.total_price) }}
            </td>
            <td class="px-6 py-4 text-right">
              <a
                :href="`/paper/disbursement-paper?tid=${tour_id}&cid=${item.no}`"
                class="font-medium text-primary-400 dark:text-primary-400 hover:underline mr-5"
                >ดูใบเบิกเงินสดย่อย</a
              >
              <a
                @click="onOpenCreateClearDialog(item.no)"
                class="font-medium text-purple-400 dark:text-purple-400 hover:underline mr-5"
                >ออกใบเคลียร์</a
              >
              <a-popconfirm
                title="คุณแน่ใจ?"
                ok-text="ยืนยัน"
                cancel-text="ยกเลิก"
                @confirm="onDeleteEstimate(item.id)"
                @cancel="cancel">
                <a
                  href="#"
                  class="font-medium text-red-600 dark:text-red-500 hover:underline"
                  >ลบ</a
                ></a-popconfirm
              >
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </a-modal>

  <a-modal v-model:visible="dialog" title="ฟอร์มสร้างใบแจ้งหนี้/ใบวางบิล">
    <template #footer>
      <a-button key="back" @click="dialog = false">ยกเลิก</a-button>
      <a-button
        key="submit"
        type="primary"
        :loading="loadGenBill"
        @click="generateBilling"
        >สร้าง</a-button
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

  <a-modal
    v-model:visible="dialog2"
    width="65rem"
    title="ฟอร์มสร้างใบกำกับภาษี/ใบส่งของ">
    <template #footer>
      <a-button key="back" @click="dialog2 = false">ยกเลิก</a-button>
      <a-button
        key="submit"
        type="primary"
        :loading="loadGenBill"
        @click="generateTaxInvoice"
        >สร้าง</a-button
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
          v-model="tax.tax_no"
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
          v-model:value="tax.tax_date"
          class="date-picker"
          format="DD/MM/YYYY" />
      </v-col>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >กำหนดการชำระ</label
        >
        <a-date-picker
          :locale="locale"
          style="z-index: 999"
          v-model:value="tax.tax_pay_date"
          class="date-picker"
          format="DD/MM/YYYY" />
      </v-col>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >สาขา (ถ้าไม่กรอกจะเป็นสำนักงานใหญ่)</label
        >
        <input
          type="text"
          id="base-input"
          v-model="tax.tax_branch"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" />
      </v-col>
    </v-row>
  </a-modal>

  <a-modal
    v-model:visible="create_clear_dialog"
    width="90rem"
    title="ออกใบเคียร์ประมาณการเงินสดย่อย">
    <template #footer>
      <a-row justify="space-between">
        <a-col>
          <a-label style="margin-right: 1rem">ยอดเงินประมาณการ</a-label
          ><a-tag color="cyan"
            >{{
              moni_clear_total_predict.toLocaleString({
                minimumFractionDigits: 2,
              })
            }}
            .-</a-tag
          ><a-divider type="vertical"></a-divider>
          <a-label style="margin-right: 1rem">ยอดสุทธิ</a-label
          ><a-tag color="success">{{ moni_clear_total }}</a-tag
          ><a-divider type="vertical"></a-divider>
          <a-label style="margin-right: 1rem">ยอดเงินเบิกเพิ่ม</a-label
          ><a-tag color="orange">{{ moni_clear_total_give_me }}</a-tag
          ><a-divider type="vertical"></a-divider>
          <a-label style="margin-right: 1rem">ยอดเงินส่งคืน</a-label
          ><a-tag color="purple">{{ moni_clear_total_return }}</a-tag>
        </a-col>
        <a-col>
          <a-button key="back" @click="create_clear_dialog = false"
            >ยกเลิก</a-button
          >
          <a-button key="submit" type="primary" @click="onCreateClear"
            >สร้าง</a-button
          ></a-col
        ></a-row
      >
    </template>
    <v-row>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >ผู้ขอเคลียร์</label
        >
        <input
          type="text"
          id="base-input"
          v-model="clear.person_clear"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" />
      </v-col>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">
          ฝ่าย/แผนก</label
        >
        <input
          type="text"
          id="base-input"
          v-model="clear.department"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" />
      </v-col>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >อ้างอิงถึงเลขที่ใบขอเคลียร์</label
        >
        <input
          type="text"
          id="base-input"
          v-model="clear.pefer_no_example_pay"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" />
      </v-col>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >รายละเอียดโครงการ/กิจกรรม</label
        >
        <input
          type="text"
          id="base-input"
          v-model="clear.tour_desc"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" />
      </v-col>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >ผู้อนุมัติ (แบบอิเล็กทอรนิกส์)
        </label>
        <input
          type="text"
          id="base-input"
          v-model="clear.approve_user"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" />
      </v-col>
    </v-row>
    <a-divider> รายการที่ขอเบิก </a-divider>
    <v-row
      ><v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >วันที่ใบเสร็จ
        </label>
        <a-date-picker
          :locale="locale"
          v-model:value="clear_date"
          class="date-picker"
          format="DD/MM/YYYY" /></v-col
      ><v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >รายละเอียดค่าใช้จ่าย
        </label>
        <input
          type="text"
          id="base-input"
          v-model="clear_desc"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" />
      </v-col>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >เลขที่ใบเสร็จ
        </label>
        <input
          type="text"
          id="base-input"
          v-model="clear_no"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" />
      </v-col>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >เลขประจำตัวผู้เสียภาษี
        </label>
        <input
          type="text"
          id="base-input"
          v-model="clear_tax_id"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" />
      </v-col>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >ยอดสุทธิ
        </label>
        <input
          type="number"
          id="base-input"
          v-model.number="clear_total"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" />
      </v-col>
      <v-col>
        <label
          for="base-input"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >รายการ</label
        >
        <select
          style="height: 33.6px"
          v-model="clear_of"
          class="block w-full p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 sm:text-xs focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
          <option
            v-for="(item, index) in est_de_ls"
            :key="index"
            :value="item.id">
            {{ item.desc }}
          </option>
        </select>
      </v-col>
      <v-col>
        <br />
        <v-btn style="width: 100%" color="blue" @click="handleAddReciept"
          >เพิ่มรายการ</v-btn
        ></v-col
      ></v-row
    >
    <div class="relative overflow-x shadow-md sm:rounded-lg mt-5">
      <table
        class="w-full text-sm overflow-scroll table-auto text-left text-gray-500 dark:text-gray-400">
        <thead
          class="text-xs uppercase bg-green-800 dark:bg-gray-700"
          style="color: white">
          <tr>
            <th scope="col" class="px-6 py-3">วันที่ใบเสร็จ</th>
            <th scope="col" class="px-6 py-3">รายละเอียดค่าใช้จ่าย</th>
            <th scope="col" class="px-6 py-3">เลขที่ใบเสร็จ</th>
            <th scope="col" class="px-6 py-3">เลขประจำตัวผู้เสียภาษี</th>
            <th scope="col" class="px-6 py-3 text-right">ยอดสุทธิ</th>
          </tr>
        </thead>
        <tbody v-for="(item, index) in est_de_ls">
          <tr
            class="bg-gray-100 border-b dark:bg-gray-800 dark:border-gray-700"
            :key="index">
            <th class="px-6 py-4 w-40">
              หัวข้ออ้างอิง<br />ไม่แสดงในใบเคลียร์
            </th>
            <td class="px-6 py-4">{{ item.desc }}</td>
            <td class="px-6 py-4">-</td>
            <td class="px-6 py-4">-</td>
            <td class="px-6 py-4 text-right">
              {{
                Number(item.total).toLocaleString({ minimumFractionDigits: 2 })
              }}
              .-
            </td>
          </tr>
          <tr
            class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600"
            v-for="(item, index) in item.clearance"
            :key="index">
            <th class="px-6 py-4">{{ item.clear_date }}</th>
            <td class="px-6 py-4">{{ item.clear_desc }}</td>
            <td class="px-6 py-4">{{ item.clear_no }}</td>
            <td class="px-6 py-4">{{ item.clear_tax_id }}</td>
            <td class="px-6 py-4 text-right">
              {{
                Number(item.clear_total).toLocaleString({
                  minimumFractionDigits: 2,
                })
              }}.-
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </a-modal>

  <a-modal v-model:visible="resturaunt_dialog" width="58rem" title="ร้านอาหาร">
    <template #footer>
      <a-button key="back" @click="resturaunt_dialog = false">ปิด</a-button>
    </template>
    <a-row justify="space-between"
      ><a-col
        ><a-input
          style="width: 50rem"
          v-model:value="resturaunt_name"
          placeholder="ชื่อร้านอาหาร" /></a-col
      ><a-col>
        <a-button type="primary" @click="handleAddResturaunt"
          >เพิ่ม</a-button
        ></a-col
      ></a-row
    >
    <a-divider> รายการร้านอาหาร </a-divider>
    <div class="relative overflow-x shadow-md sm:rounded-lg">
      <table
        class="w-full text-sm overflow-scroll table-auto text-left text-gray-500 dark:text-gray-400">
        <thead
          class="text-xs uppercase bg-green-800 dark:bg-gray-700"
          style="color: white">
          <tr>
            <th scope="col" class="px-6 py-3">ร้าน</th>
            <th scope="col" class="px-6 py-3">คอมเมนต์</th>
            <th scope="col" class="px-6 py-3">เพิ่มคอมเมนท์</th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600"
            v-for="(item, index) in resturaunt_ls.restaurants"
            :key="index">
            <th
              scope="row"
              class="px-6 font-medium text-gray-900 whitespace-nowrap dark:text-white">
              {{ item.name }}
            </th>
            <td class="px-6">{{ item.comment }}</td>
            <td class="px-6 py-2" style="width: 15rem">
              <a-button
                type="dashed"
                @click="openCommentDialog(item.name)"
                style="margin-right: 0.5rem"
                >คอมเมนท์</a-button
              ><a-button
                type="danger"
                @click="handleDeleteResturaunt(item.name)"
                >ลบ</a-button
              >
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </a-modal>
  <a-modal v-model:visible="comment_dialog" title="เพิ่มคอมเม้นต์">
    <template #footer>
      <a-button key="back" @click="comment_dialog = false">ปิด</a-button>
    </template>
    <a-row justify="space-between"
      ><a-col
        ><a-input
          style="width: 25rem"
          v-model:value="comment"
          placeholder="ข้อความ" /></a-col
      ><a-col>
        <a-button type="primary" @click="handleAddComment"
          >บันทึก</a-button
        ></a-col
      ></a-row
    >
  </a-modal>

  <a-modal
    v-model:visible="attraction_dialog"
    width="58rem"
    title="สถานที่ท่องเที่ยว">
    <template #footer>
      <a-button key="back" @click="attraction_dialog = false">ปิด</a-button>
    </template>
    <a-row justify="space-between"
      ><a-col
        ><a-input
          style="width: 50rem"
          v-model:value="attraction_name"
          placeholder="ชื่อสถานที่ท่องเที่ยว" /></a-col
      ><a-col>
        <a-button type="primary" @click="handleAddAttraction"
          >เพิ่ม</a-button
        ></a-col
      ></a-row
    >
    <a-divider> รายการสถานที่ท่องเที่ยว </a-divider>
    <div class="relative overflow-x shadow-md sm:rounded-lg">
      <table
        class="w-full text-sm overflow-scroll table-auto text-left text-gray-500 dark:text-gray-400">
        <thead
          class="text-xs uppercase bg-green-800 dark:bg-gray-700"
          style="color: white">
          <tr>
            <th scope="col" class="px-6 py-3">ชื่อ</th>
            <th scope="col" class="px-6 py-3">คอมเมนต์</th>
            <th scope="col" class="px-6 py-3">เพิ่มคอมเมนท์</th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600"
            v-for="(item, index) in attraction_ls.attractions"
            :key="index">
            <th
              scope="row"
              class="px-6 font-medium text-gray-900 whitespace-nowrap dark:text-white">
              {{ item.name }}
            </th>
            <td class="px-6">{{ item.comment }}</td>
            <td class="px-6 py-2" style="width: 15rem">
              <a-button
                type="dashed"
                style="margin-right: 0.5rem"
                @click="openAttractionCommentDialog(item.name)"
                >คอมเมนท์</a-button
              >
              <a-button type="danger" @click="handleDeleteAttraction(item.name)"
                >ลบ</a-button
              >
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </a-modal>
  <a-modal v-model:visible="attraction_comment_dialog" title="เพิ่มคอมเม้นต์">
    <template #footer>
      <a-button key="back" @click="attraction_comment_dialog = false"
        >ปิด</a-button
      >
    </template>
    <a-row justify="space-between"
      ><a-col
        ><a-input
          style="width: 25rem"
          v-model:value="attraction_comment"
          placeholder="ข้อความ" /></a-col
      ><a-col>
        <a-button type="primary" @click="handleAddAttractionComment"
          >บันทึก</a-button
        ></a-col
      ></a-row
    >
  </a-modal>

  <a-modal v-model:visible="direction_dialog" width="58rem" title="เส้นทาง">
    <template #footer>
      <a-button key="back" @click="direction_dialog = false">ปิด</a-button>
    </template>
    <a-row justify="space-between"
      ><a-col
        ><a-input
          style="width: 50rem"
          v-model:value="direction_name"
          placeholder="ชื่อเส้นทาง" /></a-col
      ><a-col>
        <a-button type="primary" @click="handleAddDirections"
          >เพิ่ม</a-button
        ></a-col
      ></a-row
    >
    <a-divider> รายการเส้นทาง </a-divider>
    <div class="relative overflow-x shadow-md sm:rounded-lg">
      <table
        class="w-full text-sm overflow-scroll table-auto text-left text-gray-500 dark:text-gray-400">
        <thead
          class="text-xs uppercase bg-green-800 dark:bg-gray-700"
          style="color: white">
          <tr>
            <th scope="col" class="px-6 py-3">ชื่อ</th>
            <th scope="col" class="px-6 py-3">คอมเมนต์</th>
            <th scope="col" class="px-6 py-3">เพิ่มคอมเมนท์</th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600"
            v-for="(item, index) in direction_ls.directions"
            :key="index">
            <th
              scope="row"
              class="px-6 font-medium text-gray-900 whitespace-nowrap dark:text-white">
              {{ item.name }}
            </th>
            <td class="px-6">{{ item.comment }}</td>
            <td class="px-6 py-2" style="width: 15rem">
              <a-button
                type="dashed"
                style="margin-right: 0.5rem"
                @click="openDirectionsCommentDialog(item.name)"
                >คอมเมนท์</a-button
              >
              <a-button type="danger" @click="handleDeleteDirections(item.name)"
                >ลบ</a-button
              >
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </a-modal>

  <a-modal v-model:visible="direction_comment_dialog" title="เพิ่มคอมเม้นต์">
    <template #footer>
      <a-button key="back" @click="direction_comment_dialog = false"
        >ปิด</a-button
      >
    </template>
    <a-row justify="space-between"
      ><a-col
        ><a-input
          style="width: 25rem"
          v-model:value="direction_comment"
          placeholder="ข้อความ" /></a-col
      ><a-col>
        <a-button type="primary" @click="handleAddDirectionComment"
          >บันทึก</a-button
        ></a-col
      ></a-row
    >
  </a-modal>

  <a-modal v-model:visible="tour_comment_dialog" title="เพิ่มคอมเม้นต์">
    <template #footer>
      <a-button key="back" @click="tour_comment_dialog = false">ปิด</a-button>
    </template>
    <a-row justify="space-between"
      ><a-col
        ><a-input
          style="width: 25rem"
          v-model:value="tour_comment"
          placeholder="ข้อความ" /></a-col
      ><a-col>
        <a-button type="primary" @click="handleUpdateComments"
          >บันทึก</a-button
        ></a-col
      ></a-row
    >
  </a-modal>

  <a-modal
    v-model:visible="hotel_comment_dialog"
    width="58rem"
    title="รายการโรงแรม">
    <template #footer>
      <a-button key="back" @click="hotel_comment_dialog = false">ปิด</a-button>
    </template>
    <div class="relative overflow-x shadow-md sm:rounded-lg">
      <table
        class="w-full text-sm overflow-scroll table-auto text-left text-gray-500 dark:text-gray-400">
        <thead
          class="text-xs uppercase bg-green-800 dark:bg-gray-700"
          style="color: white">
          <tr>
            <th scope="col" class="px-6 py-3">ชื่อ</th>
            <th scope="col" class="px-6 py-3">คอมเมนต์</th>
            <th scope="col" class="px-6 py-3">เพิ่มคอมเมนท์</th>
          </tr>
        </thead>
        <tbody>
          <tr
            class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600"
            v-for="(item, index) in hotels_ls"
            :key="index">
            <th
              scope="row"
              class="px-6 font-medium text-gray-900 whitespace-nowrap dark:text-white">
              {{ item.name }}
            </th>
            <td class="px-6">{{ item.comments }}</td>
            <td class="px-6 py-2" style="width: 15rem">
              <a-button
                type="dashed"
                style="margin-right: 0.5rem"
                @click="openHotelCommentDialog(item.id)"
                >คอมเมนท์</a-button
              >
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </a-modal>

  <a-modal v-model:visible="hotel_select_dialog" title="เพิ่มคอมเม้นต์">
    <template #footer>
      <a-button key="back" @click="hotel_select_dialog = false">ปิด</a-button>
    </template>
    <a-row justify="space-between"
      ><a-col
        ><a-input
          style="width: 25rem"
          v-model:value="hotel_comment"
          placeholder="ข้อความ" /></a-col
      ><a-col>
        <a-button type="primary" @click="handleUpdateCommentsHotel"
          >บันทึก</a-button
        ></a-col
      ></a-row
    >
  </a-modal>

  <v-layout>
    <v-navigation-drawer
      v-model="tools"
      location="bottom"
      temporary
      style="height: 23rem">
      <v-list density="compact" nav>
        <v-list-item
          title="ลบทัวร์"
          style="color: red"
          @click="handleDelete(tour_data.name)"
          value="1"
          href="/"></v-list-item>
        <v-list-item
          title="จัดการร้านอาหาร"
          @click="resturaunt_dialog = true"
          value="2"></v-list-item>
        <v-list-item
          title="จัดการสถานที่ท่องเที่ยว"
          @click="attraction_dialog = true"
          value="3"></v-list-item>
        <v-list-item
          title="จัดการเส้นทาง"
          @click="direction_dialog = true"
          value="4"></v-list-item>
        <v-list-item
          title="คอมเมนต์โรงแรม"
          @click="hotel_comment_dialog = true"
          value="8"></v-list-item>
        <v-list-item
          title="จัดการใบเบิกค่าใช้จ่าย"
          @click="list_cash = true"
          value="5"></v-list-item>
        <v-list-item
          title="จัดการใบเสนอราคา"
          @click="list_quo = true"
          value="6"></v-list-item>
        <v-list-item
          title="จัดการข้อมูลลูกทัวร์"
          @click="$router.push(`/addusertour/${tour_id}`)"
          value="7"></v-list-item>
      </v-list>
    </v-navigation-drawer>
  </v-layout>
</template>

<script>
import Swal from "sweetalert2/dist/sweetalert2.js";
import { DownOutlined } from "@ant-design/icons-vue";
import {
  read_all_data,
  read_one_data,
  delete_data,
  create_data,
  genRanDec,
  api,
} from "~~/services/pyapi";
import { useDisplay } from "vuetify";
import locale from "ant-design-vue/es/date-picker/locale/th_TH";
import dayjs from "dayjs";
import buddhistEra from "dayjs/plugin/buddhistEra";
import axios from "axios";
dayjs.extend(buddhistEra);
export default {
  setup() {
    const formatter = new Intl.NumberFormat("th-TH", {
      minimumFractionDigits: 2,
    });
    return {
      formatter,
      locale,
      DownOutlined,
    };
  },
  async mounted() {
    const { mobile } = useDisplay();
    this.isMobile = mobile.value;
    this.tour_id = this.$route.params.tourdataid;
    this.tour_data = await read_one_data("tour", this.tour_id);
    this.hotels_ls = await read_all_data("hotels?tour_id=" + this.tour_id);
    this.members_ls = await read_all_data("members?tour_id=" + this.tour_id);
    const quo_res = await read_all_data(`quotations?tour_id=${this.tour_id}`);
    const bill_res = await read_all_data(`billings?tour_id=${this.tour_id}`);
    const tax_res = await read_all_data(`taxes?tour_id=${this.tour_id}`);
    this.estimate_ls = await read_all_data(`estimates?tour_id=${this.tour_id}`);
    this.quo_ls = quo_res;
    quo_res.length ? (this.haveQuotation = true) : (this.haveQuotation = false);
    bill_res.length ? (this.haveBilling = true) : (this.haveBilling = false);
    tax_res.length ? (this.haveTax = true) : (this.haveTax = false);
    this.loading = false;

    this.billing.billing_note_no = genRanDec(7);
    this.billing.billing_note_date = dayjs(new Date());

    this.tax.tax_no = genRanDec(7);
    this.tax.tax_date = dayjs(new Date());

    this.resturaunt_ls = await read_all_data(
      `tour_get_restaurants/${this.tour_id}`
    );
    this.attraction_ls = await read_all_data(
      `tour_get_attractions/${this.tour_id}`
    );

    this.direction_ls = await read_all_data(
      `tour_get_directions/${this.tour_id}`
    );
  },
  methods: {
    handleAddResturaunt() {
      api
        .patch(
          `tour/${this.tour_id}/add_restaurant?restaurant=${this.resturaunt_name}`
        )
        .then(async (res) => {
          this.resturaunt_ls = await read_all_data(
            `tour_get_restaurants/${this.tour_id}`
          );
          this.resturaunt_name = "";
        });
    },
    onOpenCreateClearDialog(item) {
      this.est_no = item;
      this.clear.pefer_no_example_pay = item;
      read_all_data(`estimate_lists?no=${item}`).then((res) => {
        this.est_de_ls = res;
        this.moni_clear_total_predict = res.reduce((a, b) => a + b.total, 0);
      });
      this.moni_clear_total_predict = 0;
      this.moni_clear_total = 0;
      this.moni_clear_total_give_me = 0;
      this.moni_clear_total_return = 0;
      this.create_clear_dialog = true;
    },
    onOpenBillDialog(item) {
      this.quo_id = item.id;
      this.customer_id = item.customer_code;
      this.dialog = true;
    },
    onOpenTaxDialog(item) {
      this.quo_id = item.id;
      this.customer_id = item.customer_code;
      this.dialog2 = true;
    },
    onDeleteEstimate(id) {
      delete_data("estimate", id).then((res) => {
        this.estimate_ls = this.estimate_ls.filter((item) => item.id != id);
        this.$message.success("ลบข้อมูลเรียบร้อยแล้ว");
      });
    },
    onDeleteQuotation(id) {
      delete_data("quotation", id).then((res) => {
        this.quo_ls = this.quo_ls.filter((item) => item.id != id);
        this.$message.success("ลบข้อมูลเรียบร้อยแล้ว");
      });
    },
    validateQuotation() {
      if (this.tour_program.price_per_unit == 0) {
        this.$message.error("กรุณากรอกราคาต่อหน่วย");
        return false;
      }
      if (this.tour_program.discount < 0) {
        this.$message.error("กรุณากรอกส่วนลด");
        return false;
      }
      if (this.tour_program.tax == "") {
        this.$message.error("กรุณากรอกภาษี");
        return false;
      }
      return true;
    },
    onCreateQuotation() {
      let total = this.tour_data.price;
      var amount = 0;
      if (this.tour_data.tax == "7%") {
        amount = total + total * 0.07;
      } else if (this.tour_data.tax == "9%") {
        amount = total + total * 0.09;
      }
      this.tour_program.loading = true;
      const qid = genRanDec(13);
      const payload = {
        tour_id: this.tour_id,
        quo_id: qid,
        code: `Q-${genRanDec(10)}`,
        name: this.tour_data.name,
        desc: this.tour_data.program_name,
        qty: 1,
        unit: "ทัวร์",
        price_per_unit: this.tour_data.price,
        discount: 0,
        tax: this.tour_data.tax,
        amount: amount,
      };
      create_data("product", payload).then(() => {
        this.tour_program.loading = false;
        this.$message.info("ไปสู่หน้าสร้างรายละเอียดใบเสนอราคา");
        this.$router.push(`/qpform/${this.tour_id}?cid=${qid}`);
      });
    },
    handleDelete(name) {
      Swal.fire({
        title: "คุณกำลังจะลบทัวร์",
        html: `ทัวร์ <span style="color: red">${name}</span> <br/>ข้อมูลที่เกี่ยวข้องจะถูกลบอย่างถาวร`,
        icon: "question",
        confirmButtonText: "ยืนยัน",
        showCancelButton: true,
        cancelButtonText: "ยกเลิก",
        focusConfirm: false,
        showLoaderOnConfirm: true,
        preConfirm: () => {
          return delete_data("tour", this.tour_id)
            .then(() => {
              delete_data(`hotel?tid=${this.tour_id}`).then(() => {
                delete_data(`member?tid=${this.tour_id}`).then(() => {
                  this.$router.push("/");
                });
              });
            })
            .catch((error) => {
              Swal.showValidationMessage(`Request failed: ${error}`);
            });
        },
      });
    },
    validateBilling() {
      if (
        this.billing.billing_note_no == "" ||
        this.billing.billing_note_date == "" ||
        this.billing.billing_note_fax == ""
      ) {
        this.$message.error("กรุณากรอกข้อมูลให้ครบถ้วน");
        return false;
      } else {
        return true;
      }
    },
    generateBilling() {
      if (this.validateBilling()) {
        this.loadGenBill = true;
        const payload = {
          tour_id: this.tour_id,
          quo_id: this.quo_id,
          no: this.billing.billing_note_no,
          date: dayjs(this.billing.billing_note_date).format("DD/MM/BBBB"),
          fax: this.billing.billing_note_fax,
        };
        create_data("billing", payload).then(() => {
          axios
            .patch(
              `https://nilpaka-be.vercel.app/api/quotation-have-bill/${this.quo_id}`
            )
            .then(() => {
              this.loadGenBill = false;
              this.dialog = false;
              this.$message.success("สร้างใบเสนอราคาเรียบร้อย");
              this.$router.push(`/paper/billing-paper?qid=${this.quo_id}`);
            });
        });
      }
    },
    validateTax() {
      if (
        this.tax.tax_no == "" ||
        this.tax.tax_date == "" ||
        this.tax.tax_pay_date == ""
      ) {
        this.$message.error("กรุณากรอกข้อมูลให้ครบถ้วน");
        return false;
      } else {
        return true;
      }
    },
    generateTaxInvoice() {
      if (this.validateTax()) {
        this.loadGenBill = true;
        const payload = {
          tour_id: this.tour_id,
          quo_id: this.quo_id,
          no: this.tax.tax_no,
          date: dayjs(this.tax.tax_date).format("DD/MM/BBBB"),
          pay_date: dayjs(this.tax.tax_pay_date).format("DD/MM/BBBB"),
          branch: this.tax.tax_branch,
        };
        create_data("tax", payload).then(() => {
          axios
            .patch(
              `https://nilpaka-be.vercel.app/api/quotation-have-tax/${this.quo_id}`
            )
            .then(() => {
              this.loadGenBill = false;
              this.dialog2 = false;
              this.$message.success("สร้างใบเสนอราคาเรียบร้อย");
              this.$router.push(`/paper/tax-paper?qid=${this.quo_id}`);
            });
        });
      }
    },

    onCreateClear() {
      this.clear.date = dayjs().format("DD/MM/BBBB");
      this.clear.no = genRanDec(13);
      this.clear.tour_name = this.tour_data.name;
      this.clear.desc_ls = this.est_de_ls;
      this.clear.total_predict = this.est_de_ls.reduce(
        (a, b) => a + b.total,
        0
      );
      const sum = this.est_de_ls.map((item) => {
        return item.clearance.reduce((a, b) => a + b.clear_total, 0);
      });
      this.clear.total = sum.reduce((partialSum, a) => partialSum + a, 0);
      const json = JSON.stringify(this.clear);
      this.$router.push(`/paper/estimate_clear-paper?data=${json}`);
    },
    openCommentDialog(resturaunt_name) {
      this.comment_dialog = true;
      this.selected_resturaunt = resturaunt_name;
    },
    handleAddComment() {
      api
        .patch(
          `/tour/${this.tour_id}/add_restaurant_comment?restaurant=${this.selected_resturaunt}&comment=${this.comment}`
        )
        .then(async () => {
          this.comment_dialog = false;
          this.resturaunt_ls = await read_all_data(
            `tour_get_restaurants/${this.tour_id}`
          );
          this.comment = "";
          this.$message.success("เพิ่มความคิดเห็นเรียบร้อย");
        });
    },
    handleAddAttraction() {
      api
        .patch(
          `tour/${this.tour_id}/add_attraction?attraction=${this.attraction_name}`
        )
        .then(async (res) => {
          this.attraction_ls = await read_all_data(
            `tour_get_attractions/${this.tour_id}`
          );
          this.attraction_name = "";
        });
    },
    openAttractionCommentDialog(attraction_name) {
      this.attraction_comment_dialog = true;
      this.selected_attraction = attraction_name;
    },
    handleAddAttractionComment() {
      api
        .patch(
          `/tour/${this.tour_id}/add_attraction_comment?attraction=${this.selected_attraction}&comment=${this.attraction_comment}`
        )
        .then(async () => {
          this.attraction_comment_dialog = false;
          this.attraction_ls = await read_all_data(
            `tour_get_attractions/${this.tour_id}`
          );
          this.attraction_comment = "";
          this.$message.success("เพิ่มความคิดเห็นเรียบร้อย");
        });
    },
    handleDeleteResturaunt(resturaunt_name) {
      api
        .patch(
          `/tour/${this.tour_id}/remove_restaurant?restaurant=${resturaunt_name}`
        )
        .then(async () => {
          this.resturaunt_ls = await read_all_data(
            `tour_get_restaurants/${this.tour_id}`
          );
          this.$message.success("ลบร้านอาหารเรียบร้อย");
        });
    },
    handleDeleteAttraction(attraction_name) {
      api
        .patch(
          `/tour/${this.tour_id}/remove_attraction?attraction=${attraction_name}`
        )
        .then(async () => {
          this.attraction_ls = await read_all_data(
            `tour_get_attractions/${this.tour_id}`
          );
          this.$message.success("ลบสถานที่ท่องเที่ยวเรียบร้อย");
        });
    },
    handleAddDirections() {
      api
        .patch(
          `tour/${this.tour_id}/add_direction?direction=${this.direction_name}`
        )
        .then(async (res) => {
          this.direction_ls = await read_all_data(
            `tour_get_directions/${this.tour_id}`
          );
          console.log(this.directions_ls);
          this.directions_name = "";
        });
    },
    openDirectionsCommentDialog(directions_name) {
      this.direction_comment_dialog = true;
      this.selected_direction = directions_name;
    },
    handleAddDirectionComment() {
      api
        .patch(
          `/tour/${this.tour_id}/add_direction_comment?direction=${this.selected_direction}&comment=${this.direction_comment}`
        )
        .then(async () => {
          this.direction_comment_dialog = false;
          this.direction_ls = await read_all_data(
            `tour_get_directions/${this.tour_id}`
          );
          this.direction_comment = "";
          this.$message.success("เพิ่มความคิดเห็นเรียบร้อย");
        });
    },
    handleDeleteDirections(directions_name) {
      api
        .patch(
          `/tour/${this.tour_id}/remove_direction?direction=${directions_name}`
        )
        .then(async () => {
          this.direction_ls = await read_all_data(
            `tour_get_directions/${this.tour_id}`
          );
          this.$message.success("ลบทิศทางเดินเรียบร้อย");
        });
    },
    handleAddReciept() {
      const payload = {
        clear_date: dayjs(this.clear_date).format("DD/MM/BBBB"),
        clear_desc: this.clear_desc,
        clear_no: this.clear_no,
        clear_tax_id: this.clear_tax_id,
        clear_total: this.clear_total,
      };
      const objIndex = this.est_de_ls.findIndex(
        (obj) => obj.id == this.clear_of
      );
      this.est_de_ls[objIndex].clearance
        ? null
        : (this.est_de_ls[objIndex].clearance = []);
      this.est_de_ls[objIndex].clearance.push(payload);

      const sum = this.est_de_ls.map((item) => {
        if (item.clearance) {
          return item.clearance.reduce((partialSum, a) => {
            return partialSum + a.clear_total;
          }, 0);
        } else {
          return 0;
        }
      });

      this.moni_clear_total = sum.reduce((partialSum, a) => partialSum + a, 0);

      if (this.moni_clear_total_predict - this.moni_clear_total <= 0) {
        this.moni_clear_total_return = 0;
      } else {
        this.moni_clear_total_return =
          this.moni_clear_total_predict - this.moni_clear_total;
      }
      if (this.moni_clear_total - this.moni_clear_total_predict <= 0) {
        this.moni_clear_total_give_me = 0;
      } else {
        this.moni_clear_total_give_me =
          this.moni_clear_total - this.moni_clear_total_predict;
      }
    },
    handleUpdateComments() {
      api
        .patch(
          `/tour_add_comments/${this.tour_id}?comments=${this.tour_comment}`
        )
        .then(async () => {
          this.tour_comment_dialog = false;
          this.tour_data = await read_one_data("tour", this.tour_id);
          this.$message.success("อัพเดทความคิดเห็นเรียบร้อย");
        });
    },
    openHotelCommentDialog(id) {
      this.hotel_selected_id = id;
      this.hotel_select_dialog = true;
    },
    handleUpdateCommentsHotel() {
      api
        .patch(
          `/hotel_add_comment/${this.hotel_selected_id}?comment=${this.hotel_comment}`
        )
        .then(async () => {
          this.hotel_select_dialog = false;
          this.hotels_ls = await read_all_data(
            "hotels?tour_id=" + this.tour_id
          );
          this.$message.success("อัพเดทความคิดเห็นเรียบร้อย");
        });
    },
    openMemberDetailDialog(item) {
      this.selected_member = item;
      this.member_detail_dialog = true;
    },
    handleUpdateMemberComment() {
      api
        .patch(
          `/member_add_vip_u/${this.selected_member.id}?vip_u=${this.member_comment}`
        )
        .then(async () => {
          this.member_detail_dialog = false;
          this.member_comment = "";
          this.members_ls = await read_all_data(
            "members?tour_id=" + this.tour_id
          );
          this.$message.success("อัพเดทความคิดเห็นเรียบร้อย");
        });
    },
  },
  data() {
    return {
      tour_id: "",
      quo_id: "",
      tour_data: "",
      customer_id: "",
      loading: true,
      haveQuotation: false,
      haveBilling: false,
      haveTax: false,
      dialog: false,
      dialog2: false,
      list_quo: false,
      list_cash: false,
      create_clear_dialog: false,
      loadGenBill: false,
      members_ls: [],
      hotels_ls: [],
      quo_ls: [],
      estimate_ls: [],
      est_de_ls: [],
      tour_program: {
        loading: false,
        price_per_unit: 0,
        discount: 0,
        tax: "",
      },
      billing: {
        billing_note_no: "",
        billing_note_date: "",
        billing_note_fax: "",
      },
      tax: {
        tax_no: "",
        tax_date: "",
        tax_pay_date: "",
        tax_branch: "",
      },
      est_no: "",
      clear: {
        no: "",
        date: "",
        person_clear: "",
        pefer_no_example_pay: "",
        department: "",
        tour_name: "",
        date_no_receipt: "",
        tax_receipt: "",
        desc_ls: [],
        total: 0,
        total_predict: 0,
        total_return: 0,
        total_give_me: 0,
        tour_desc: "",
        approve_user: "",
      },
      resturaunt_dialog: false,
      resturaunt_name: "",
      resturaunt_ls: [],
      comment: "",
      comment_dialog: false,
      selected_resturaunt: "",
      attraction_ls: [],
      attraction_dialog: false,
      attraction_name: "",
      attraction_comment: "",
      attraction_comment_dialog: false,
      selected_attraction: "",
      direction_ls: [],
      direction_dialog: false,
      direction_name: "",
      direction_comment: "",
      direction_comment_dialog: false,
      selected_direction: "",
      clear_reciept_ls: [],
      clear_date: "",
      clear_desc: "",
      clear_no: "",
      clear_tax_id: "",
      clear_total: 0,
      clear_of: "",
      isMobile: false,
      tools: false,
      tour_comment_dialog: false,
      tour_comment: "",
      hotel_comment_dialog: false,
      hotel_comment: "",
      hotel_select_dialog: false,
      hotel_selected_id: "",
      moni_clear_total_predict: 0,
      moni_clear_total: 0,
      moni_clear_total_return: 0,
      moni_clear_total_give_me: 0,
      selected_member: "",
      member_detail_dialog: false,
      member_comment: "",
    };
  },
};
</script>

<style scoped>
.table-row-hover {
  cursor: pointer;
  background-color: rgb(255, 255, 255);
  transition: 0.2s;
}

.table-row-hover:hover {
  background-color: rgb(236, 236, 236);
  transition: 0.2s;
}
.date-picker {
  height: 33.6px;
  background-color: #f9fafb;
  border-radius: 0.4rem;
  width: 100%;
}
</style>
