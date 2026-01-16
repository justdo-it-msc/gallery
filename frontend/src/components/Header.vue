<script setup>
import { logout } from "@/services/accountService";
import { useAccountStore } from "@/stores/account";
import { useRouter } from "vue-router";

// 계정 스토어
const accountStore = useAccountStore();

// 라우터 객체
const router = useRouter();

// 로그아웃
const logoutAccount = async () => {
  // 서버에 로그아웃 요청
  await logout();

  // 서버 응답과 관계없이 클라이언트 상태 초기화
  // 스토어 초기화
  accountStore.setAccessToken("");
  accountStore.setLoggedIn(false);

  // refreshToken 쿠키 삭제 (여러 방법으로 시도)
  document.cookie =
    "refreshToken=; path=/; expires=Thu, 01 Jan 1970 00:00:00 GMT";
  document.cookie =
    "refreshToken=; path=/; expires=Thu, 01 Jan 1970 00:00:00 GMT; domain=" +
    window.location.hostname;

  // 홈으로 이동
  await router.push("/");
};
</script>

<template>
  <header>
    <div class="navbar navbar-dark bg-dark text-white shadow-sm">
      <div class="container">
        <router-link to="/" class="navbar-brand">
          <strong>Gallery</strong>
        </router-link>
        <div class="menus d-flex gap-3">
          <template v-if="!accountStore.loggedIn">
            <router-link to="/login">로그인</router-link>
            <router-link to="/join">회원가입</router-link>
          </template>
          <template v-else>
            <a @click="logoutAccount()">로그아웃</a>
            <router-link to="/orders">주문내역</router-link>
            <router-link to="/cart">장바구니</router-link>
          </template>
        </div>
      </div>
    </div>
  </header>
</template>

<style lang="scss">
header {
  .menus {
    a {
      cursor: pointer;
      color: #fff;
      text-decoration: none;
    }
  }
}
</style>
