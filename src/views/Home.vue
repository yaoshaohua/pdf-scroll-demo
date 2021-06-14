<template>
  <div class="home">
    <header class="logo">logo</header>
    <main class="main">
      <nav class="nav_wrapper" ref="nav_wrapper">
        <div>
          <h3>文档缩略图</h3>
          <p>页数：{{ count }}</p>
        </div>
        <ul class="nav_list" ref="nav_list">
          <li
            v-for="(item, index) in imgList"
            :key="item"
            :class="['nav_item', { active: index === currentIndex }]"
            @click="checkNav(index)"
          >
            <img :src="item" alt="缩略图" class="nav" />
            <p class="page_number">{{ index + 1 }}</p>
          </li>
        </ul>
      </nav>
      <section class="section">
        <header class="pdf_title">pdf的名字</header>
        <main class="pdf_wrapper" ref="pdf_wrapper" @scroll="throttledScroll">
          <ul class="pdf_list" ref="pdf_list">
            <li v-for="item in imgList" :key="item" class="pdf_item">
              <img :src="item" alt="preview" class="pdf" />
            </li>
          </ul>
        </main>
        <footer class="pdf_pagination">{{ currentIndex }}/{{ count }}</footer>
      </section>
      <aside class="aside">pdf列表</aside>
    </main>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import _ from "lodash";

@Component({})
export default class Home extends Vue {
  private imgList = [
    "http://192.168.31.208:8080/pdf/pdf_00.png",
    "http://192.168.31.208:8080/pdf/pdf_01.png",
    "http://192.168.31.208:8080/pdf/pdf_02.png",
    "http://192.168.31.208:8080/pdf/pdf_03.png",
    "http://192.168.31.208:8080/pdf/pdf_04.png",
    "http://192.168.31.208:8080/pdf/pdf_05.png",
    "http://192.168.31.208:8080/pdf/pdf_06.png",
    "http://192.168.31.208:8080/pdf/pdf_07.png",
    "http://192.168.31.208:8080/pdf/pdf_08.png",
    "http://192.168.31.208:8080/pdf/pdf_09.png",
  ];
  private get count() {
    return this.imgList.length;
  }
  private currentIndex = 0;

  private checkNav(index: number) {
    if (index === this.currentIndex) return;
    this.currentIndex = index;
    document.getElementsByClassName("pdf_item")[index].scrollIntoView({
      behavior: "smooth",
    });
  }

  private handleScroll() {
    const pdfWrapper = this.$refs.pdf_wrapper as HTMLElement;
    if (!pdfWrapper) return;

    const { scrollTop } = pdfWrapper;
    const { height } = document
      .getElementsByClassName("pdf_item")[0]
      .getBoundingClientRect();
    const { height: navHeight } = document
      .getElementsByClassName("nav_item")[0]
      .getBoundingClientRect();

    this.currentIndex = Math.round(scrollTop / height);
    const scrollY = scrollTop * (navHeight / height);
    document.getElementsByClassName("nav_wrapper")[0].scrollTop = scrollY;
  }

  private throttledScroll = _.throttle(this.handleScroll, 20);
}
</script>
<style lang="less" scoped>
.home {
  height: 100vh;
}
.logo {
  height: 54px;
  background: #fff;
  box-shadow: 0 3px 6px 0 #ececec;
}
.main {
  height: calc(100% - 54px);
  overflow: hidden;
  display: grid;
  grid-template-columns: 170px auto 200px;
}
.nav_wrapper {
  height: 100%;
  overflow-y: auto;
}
.nav_list {
  width: 108px;
  margin: 0 auto;
}
.nav_item {
  margin: 30px auto;
  cursor: pointer;
}
.nav {
  width: 108px;
  border: 1px solid #e6e6e6;
}
.active {
  color: #2994ff;
  .nav {
    border-color: #2994ff;
  }
}
.page_number {
  margin-top: 8px;
  text-align: center;
  font-size: 12px;
}
.section {
  height: 100%;
  overflow: hidden;
  background: #f8f8f8;
}
.pdf_title {
  height: 48px;
}
.pdf_pagination {
  height: 30px;
}
.pdf_wrapper {
  height: calc(100% - 78px);
  overflow-y: auto;
}
.pdf_list {
  width: 800px;
  margin: 0 auto;
}
.pdf_item {
  border-bottom: 1px solid #ccc;
}
.pdf {
  width: 100%;
}
</style>
