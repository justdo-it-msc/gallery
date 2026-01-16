<script setup>
import { watch } from "vue";
import { useRoute } from "vue-router";
import Footer from "./components/Footer.vue";
import Header from "./components/Header.vue";
import { check } from "./services/accountService";
import { useAccountStore } from "./stores/account";

// 계정 스토어
const accountStore = useAccountStore();

// 라우트 객체
const route = useRoute();

// 로그인 여부 확인
const checkAccount = async () => {
  const res = await check();

  if (res.status === 200) {
    accountStore.setChecked(true);
    accountStore.setLoggedIn(res.data === true);
  } else {
    accountStore.setChecked(false);
  }
};

// 커스텀 훅 생성
(async function onCreated() {
  await checkAccount();
})();

// 라우트 경로가 바뀔 때마다 로그인 여부를 확인
watch(
  () => route.path,
  () => {
    checkAccount();
  }
);
</script>

<template>
  <Header />
  <main>
    <!-- 라우터 뷰 -->
    <router-view></router-view>
  </main>
  <Footer />
</template>
