<template>
  <div id="overley">
    <div class="container my-2">
      <div
        v-for="(noticeboard, index) in noticeboards"
        v-show="noticeboard.status"
        :key="index"
        class="row flex-sm-column-reverse flex-column-reverse flex-lg-row"
      >
        <div class="col-12 col-sm-12 col-md-12 col-lg-6 mt-5">
          <!-- eslint-disable-next-line vue/no-v-html -->
          <p v-html="noticeboard.description"></p>

          <a :href="noticeboard.linkbtn" target="_blank"
            ><b-button class="outline-purple mt-3"
              >สมัครอบรม คลิ๊ก!!!</b-button
            ></a
          >
        </div>
        <div class="col-12 col-sm-12 col-md-12 col-lg-6 text-center">
          <img
            :src="`https://api.unii.co.th/api/uploads/${noticeboard.image}`"
            alt=""
            width="75%"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      noticeboards: [],
    }
  },
  mounted() {
    this.getNoticeboard()
  },
  methods: {
    async getNoticeboard() {
      await this.$axios
        .$get('/api/noticeboards')
        .then((res) => {
          // console.log('zz', res.data)
          this.noticeboards = res.data
          // console.log('logNoticeboard', this.noticeboards)
        })
        .catch((error) => {
          // eslint-disable-next-line no-console
          console.log(error)
        })
      // this.noticeboards = data
    },
  },
}
</script>

<style scoped>
.btn-custom {
  background: #ffd700;
  color: #4a1e61;
}
p {
  color: #ffffff;
  font-weight: 700;
  margin-bottom: 0;
  font-weight: 0;
  font-family: serif;
}
.col-12.col-sm-12.col-md-12.col-lg-6.text-center {
  margin-bottom: 300px;
}
#noticeboard {
  background: transparent linear-gradient(180deg, #e62ae0 0%, #4a1e61 100%) 0%
    0% no-repeat padding-box;
  border: 1px solid #707070;
  opacity: 1;
}
</style>
