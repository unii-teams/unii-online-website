<template>
  <div>
    <div class="bg-img"></div>
    <div class="centered"><h1>OUR TEAM</h1></div>
    <div class="container mb-5">
      <b-card>
        <b-modal v-model="show" size="lg">
          <div class="pl-4">
            <p>{{ selected.uniiId }}</p>
            <h5>{{ selected.name }}</h5>
            <p>{{ selected.address }}</p>
            <p>{{ selected.tel }}</p>
            <b-avatar
              rounded
              :src="`${$axios.defaults.baseURL}/api/uploads/${selected.image}`"
              size="12rem"
            ></b-avatar>
          </div>

          <div class="mapstyle">
            <GoogleMap :items="selected"> </GoogleMap>
          </div>
        </b-modal>
        <template #header>
          <div class="card-header">
            <h4>ตัวแทนที่ได้รับการแต่งตั้ง Authorized Unii Center</h4>
          </div>
        </template>
        <!-- <b-form inline>
          <div>
            <b-form-select
              v-model="uniitypeselected"
              size="sm"
              class="mb-2 mr-sm-2 mb-sm-0 mx-4"
              :options="uniitypeoptions"
            ></b-form-select>
          </div>

          <div>
            <b-form-select
              v-model="cityselected"
              size="sm"
              class="mb-2 mr-sm-2 mb-sm-0 mx-4"
              :options="cityoptions"
            ></b-form-select>
          </div>

          <div>
            <b-form-input
              size="sm"
              class="mb-2 mr-sm-2 mb-sm-0 mx-4"
              type="search"
              placeholder="พิมพ์คำค้นหา เช่น สถานที่อยู่ เขต รหัสไปรษณีย์"
              aria-label="Search"
            ></b-form-input>
          </div>

          <div>
            <b-button href="#" size="sm" class="btn btn-purple" type="submit"
              >ค้นหา</b-button
            >
          </div>
        </b-form> -->

        <b-row>
          <b-col lg="6" class="my-1">
            <b-form-group
              v-slot="{ ariaDescribedby }"
              label="ค้นหาจาก"
              label-for="sort-by-select"
              label-cols-sm="3"
              label-align-sm="right"
              label-size="sm"
              class="mb-0"
            >
              <b-input-group size="sm">
                <b-form-select
                  id="sort-by-select"
                  v-model="selectedType"
                  :options="sortOptionsType"
                  :aria-describedby="ariaDescribedby"
                  class="w-75"
                  @change="getFilteredUniisType"
                >
                  <template #first>
                    <option value="">-- ทั้งหมด --</option>
                  </template>
                </b-form-select>
              </b-input-group>
            </b-form-group>
          </b-col>

          <b-col lg="6" class="my-1">
            <b-form-group
              v-slot="{ ariaDescribedby }"
              label="ค้นหาจังหวัด"
              label-for="sort-by-select"
              label-cols-sm="3"
              label-align-sm="right"
              label-size="sm"
              class="mb-0"
            >
              <b-input-group size="sm">
                <b-form-select
                  id="sort-by-select"
                  v-model="selectedProvince"
                  :options="sortOptionsCity"
                  :aria-describedby="ariaDescribedby"
                  class="w-75"
                  @change="getFilteredUniisProvince"
                >
                  <template #first>
                    <option value="">-- ทั้งหมด --</option>
                  </template>
                </b-form-select>
              </b-input-group>
            </b-form-group>
          </b-col>

          <b-col lg="6" class="my-1">
            <b-form-group
              label="พิมพ์คำค้นหา"
              label-for="filter-input"
              label-cols-sm="3"
              label-align-sm="right"
              label-size="sm"
              class="mb-0"
            >
              <b-input-group size="sm">
                <b-form-input
                  id="filter-input"
                  v-model.trim="search"
                  type="search"
                  placeholder="ค้นหา"
                  @keyup="getUnii"
                ></b-form-input>

                <!-- <b-input-group-append>
                  <b-button>ค้นหา</b-button>
                </b-input-group-append>

                <b-input-group-append class="mx-3">
                  <b-button :disabled="!filter" @click="filter = ''"
                    >Clear</b-button
                  >
                </b-input-group-append> -->
              </b-input-group>
            </b-form-group>
          </b-col>
        </b-row>

        <b-card-body>
          <div v-if="uniis.length > 0">
            <GoogleMap :items="uniis"> </GoogleMap>
          </div>
        </b-card-body>

        <div>
          <table class="table table-hover">
            <thead>
              <tr>
                <!-- <th scope="col">#</th> -->
                <th scope="col">ตำแหน่ง</th>
                <th scope="col">ชื่อ</th>
                <th scope="col">ที่อยู่</th>
                <th scope="col">เบอร์โทรศัพท์</th>
                <th scope="col">Authorized</th>
                <th scope="col">รายละเอียด</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in uniis" :key="item.id" class="overflow-auto">
                <!-- <th scope="row">1</th> -->
                <td>
                  {{ item.type }}
                </td>
                <td>
                  {{ item.name }}
                </td>
                <td>
                  {{ item.address }}
                </td>
                <td>
                  {{ item.tel }}
                </td>
                <td>
                  <b-img
                    v-if="item.authorized === 'authorized'"
                    center
                    src="../../assets/imgs/authorized/Authorized.svg"
                    alt="Center image"
                    width="50"
                  ></b-img>
                  <b-img
                    v-if="item.authorized === 'hub'"
                    center
                    src="../../assets/imgs/authorized/UniiHubCenter.png"
                    alt="Center image"
                    width="50"
                  ></b-img>
                </td>
                <td>
                  <b-link size="sm" @click="sendInfo(item)"> ดูข้อมูล </b-link>
                </td>
              </tr>
            </tbody>
          </table>
          <!-- <b-table
            sticky-header
            hover
            :items="uniis"
            :fields="fields"
            :filter="filter"
            :sort-by.sync="sortBy"
          >
            <template #cell(authorized)="row">
              <div>
                <b-img
                  v-if="row.item.authorized"
                  center
                  src="../../assets/imgs/authorized/Authorized.svg"
                  alt="Center image"
                  width="50"
                ></b-img>
              </div>
            </template>
            <template #cell(actions)="row">
              <b-link size="sm" @click="sendInfo(row.item)"> ดูข้อมูล </b-link>
            </template>
          </b-table> -->
        </div>
      </b-card>
    </div>
  </div>
</template>

<script>
import Provinces from '../uniis/Province.json'
export default {
  data() {
    return {
      uniis: [],
      type: '',
      uniitypes: [],
      provinces: Provinces.result,
      fields: [
        {
          key: 'type',
          label: 'ตำแหน่ง',
          sortable: true,
        },
        {
          key: 'name',
          label: 'ชื่อ',
          sortable: false,
        },
        {
          key: 'address',
          label: 'สาขา/ที่ตั้ง',
          sortable: false,
        },
        {
          key: 'tel',
          label: 'เบอร์โทรศัพท์',
          sortable: false,
        },
        {
          key: 'authorized',
          label: 'authorized',
          sortable: true,
        },
        {
          key: 'actions',
          label: 'แผนที่',
        },
      ],
      sortBy: '',
      sortDesc: false,
      sortDirection: 'asc',
      filter: null,
      filterOn: [],
      selected: '',
      show: false,
      selectedType: null,
      selectedProvince: null,
      search: '',
    }
  },
  computed: {
    // filterUniisByType() {
    //   return this.uniis.filter((unii) => !unii.type.indexOf(this.type))
    // },
    sortOptionsType() {
      // Create an options list from our fields
      return this.uniitypes.map((f) => {
        return { text: f.uniitype, value: f.uniitype }
      })
    },
    sortOptionsCity() {
      // Create an options list from our fields
      return this.provinces.map((f) => {
        return { text: f.prov_name, value: f.prov_name }
      })
    },
  },
  mounted() {
    this.getUnii()
    this.getUniiType()
  },
  created() {
    this.getUnii()
  },
  methods: {
    async getFilteredUniisType() {
      await this.$axios
        .$get(`/api/alluniis?type=${this.selectedType}`)
        .then((res) => {
          this.uniis = res.data
        })
    },

    async getFilteredUniisProvince() {
      await this.$axios
        .$get(`/api/alluniis?address=${this.selectedProvince}`)
        .then((res) => {
          this.uniis = res.data
        })
    },

    async getUniiType() {
      await this.$axios
        .$get('/api/uniitypes')
        .then((res) => {
          // console.log('zz', res.data)
          this.uniitypes = res.data
          // console.log('logUnii', this.uniis)
        })
        .catch((error) => {
          // eslint-disable-next-line no-console
          console.log(error)
        })
    },
    async getUnii() {
      await this.$axios
        .$get('/api/alluniis')
        .then((res) => {
          if (this.search) {
            this.uniis = res.data.filter(
              (unii) =>
                unii.type.toLowerCase().includes(this.search.toLowerCase()) ||
                unii.name.toLowerCase().includes(this.search.toLowerCase()) ||
                unii.address
                  .toLowerCase()
                  .includes(this.search.toLowerCase()) ||
                unii.tel.toLowerCase().includes(this.search.toLowerCase())
            )
          } else {
            this.uniis = res.data
          }
          // console.log('zz', res.data)
          // this.uniis = res.data
          // console.log('logUnii', this.uniis)
        })
        .catch((error) => {
          // eslint-disable-next-line no-console
          console.log(error)
        })
    },
    sendInfo(item) {
      this.selected = item
      this.show = true
    },
  },
}
</script>

<style>
html {
  scroll-behavior: smooth;
}
body {
  font-family: 'Prompt', sans-serif;
  background-color: #f2f2f2;
}
.mapstyle {
  padding-left: 20px;
  padding-right: 20px;
}
/* .card {
  margin: 20px;
  margin-top: -170px;
  height: 60em;
  border: none;
} */
.card {
  margin-top: -150px;
}
.card-header {
  background-color: #993a96;
  padding: 20px;
}
thead {
  color: #993a96;
}
h1 {
  color: #ffffff;
}
h5 {
  color: #69197b;
  font-weight: 700;
}
.centered {
  position: absolute;
  top: 25%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.btn-purple {
  background: transparent linear-gradient(180deg, #993a96 0%, #69197b 100%) 0%
    0% no-repeat padding-box;
  border-radius: 5px;
  opacity: 1;
  border: none;
  padding: 10px 40px;
}
h4 {
  color: #ffffff;
}

.bg-img {
  /* The image used */
  background-image: url('../../assets/imgs/bgunii.jpg');

  min-height: 380px;

  /* Center and scale the image nicely */
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  /* position: relative; */
}
</style>
