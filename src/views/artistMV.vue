<template>
  <div v-show="show">
    <h1>
      <img class="avatar" :src="artist.img1v1Url | resizeImage(1024)" />{{
        artist.name
      }}'s Music Videos
    </h1>
    <MvRow :mvs="mvs" subtitle="publishTime" />
    <div class="load-more">
      <ButtonTwoTone v-show="hasMore" @click.native="loadMVs" color="grey">{{
        $t("explore.loadMore")
      }}</ButtonTwoTone>
    </div>
  </div>
</template>

<script>
import { artistMv, getArtist } from "@/api/artist";
import NProgress from "nprogress";

import ButtonTwoTone from "@/components/ButtonTwoTone.vue";
import MvRow from "@/components/MvRow.vue";

export default {
  name: "artistMV",
  components: {
    MvRow,
    ButtonTwoTone,
  },
  data() {
    return {
      id: 0,
      show: false,
      hasMore: true,
      artist: {},
      mvs: [],
    };
  },
  methods: {
    loadData() {
      getArtist(this.id).then((data) => {
        this.artist = data.artist;
      });
      this.loadMVs();
    },
    loadMVs() {
      artistMv({ id: this.id, limit: 100, offset: this.mvs.length + 1 }).then(
        (data) => {
          this.mvs.push(...data.mvs);
          this.hasMore = data.hasMore;
          NProgress.done();
          this.show = true;
        }
      );
    },
  },
  created() {
    this.id = this.$route.params.id;
    this.loadData();
  },
  activated() {
    if (this.$route.params.id !== this.id) {
      this.id = this.$route.params.id;
      this.mvs = [];
      this.artist = {};
      this.show = false;
      this.hasMore = true;
      this.loadData();
    }
  },
  beforeRouteUpdate(to, from, next) {
    NProgress.start();
    this.id = to.params.id;
    this.loadData();
    next();
  },
};
</script>

<style lang="scss" scoped>
h1 {
  font-size: 42px;
  color: var(--color-text);
  .avatar {
    height: 44px;
    margin-right: 12px;
    vertical-align: -7px;
    border-radius: 50%;
    border: rgba(0, 0, 0, 0.2);
  }
}
.load-more {
  display: flex;
  justify-content: center;
}
</style>
