<template>
  <div id="video">
    <div class="container pt-5">
      <div class="text-center">
        <h1>ทำความรู้จักกับ ยูนี่ ออนไลน์</h1>
        <p>
          Disrupt ต้นทุนธุรกิจรีไซเคิล ครั้งยิ่งใหญ่
          ครั้งแรกของประเทศไทยคืนทุนไว ได้กำไรแน่นอน
        </p>
      </div>
      <div>
        <b-embed
          v-if="videos[0]"
          id="youtube"
          :src="`https://www.youtube.com/embed/${videos[0].title}?controls=0`"
          frameborder="0"
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen
        ></b-embed>
      </div>

      <div class="mt-3">
        <div v-if="videos.length > 0">
          <client-only>
            <carousel
              class="sectioncarousel"
              :autoplay="true"
              :loop="true"
              :margin="20"
              :video="true"
              :nav="false"
              :responsive="{
                0: { items: 1, nav: false },
                600: { items: 3, nav: false },
              }"
            >
              <div v-for="item in videos" :key="item.id">
                <img
                  :src="`https://img.youtube.com/vi/${item.title}/0.jpg`"
                  style="width: 100%; height: 200px"
                  alt=""
                  @click="changeVideo(item.title)"
                />
              </div>
            </carousel>
          </client-only>
        </div>
        <div class="text-center">
          <NuxtLink to="/videos/allvideos">
            <b-button size="lg" class="btn btn-purple" type="submit"
              >วีดีโอทั้งหมด</b-button
            ></NuxtLink
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      videos: [],
    }
  },
  mounted() {
    this.getVideo()
  },
  methods: {
    async getVideo() {
      await this.$axios
        .$get('/api/videos?limit=50')
        .then((res) => {
          // console.log('zz', res.data)
          this.videos = res.data
          // console.log('logVideo', this.videos)
        })
        .catch((error) => {
          // eslint-disable-next-line no-console
          console.log(error)
        })
    },
    changeVideo(videoId) {
      const ytString = 'https://www.youtube.com/embed/' + videoId
      document.getElementById('youtube').src = ytString
    },
  },
}
</script>

<style scoped>
h1 {
  color: #69197b;
  font-weight: 700;
}
</style>
