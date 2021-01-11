<template>
  <nav class="navbar">
    <div class="navbar__logo">
      <a href="/">
        <img
          class="logo"
          src="/blog/333x213px-kyh_transparent.png"
          alt
          width="40"
        />
        <span class="title">Hulkong의 블로그</span>
      </a>
    </div>

    <ul class="navbar__menu" :class="{ active: isMenuActive }">
      <li><a href="">Home</a></li>
      <li><a href="">Category</a></li>
      <li><a href="">FAQ</a></li>
    </ul>

    <AppSearchInput />

    <ul class="navbar__icons" :class="{ active: isIconsActive }">
      <li>
        <a href="https://www.facebook.com/Hulkong2" target="_blank"
          ><i class="fab fa-facebook-square"></i
        ></a>
      </li>
      <li>
        <a href="https://www.instagram.com/hulkongg/" target="_blank"
          ><i class="fab fa-instagram"></i
        ></a>
      </li>
      <li>
        <a href="https://github.com/Hulkong" target="_blank"
          ><i class="fab fa-github"></i
        ></a>
      </li>
    </ul>

    <a
      href="#"
      class="navbar__toggleBtn"
      :class="{ active: isHambuggerActive }"
      @click="clickToggleBtn"
    >
      <i class="fas fa-bars"></i>
    </a>
  </nav>
</template>

<script>
export default {
  data: () => ({
    isMenuActive: false,
    isIconsActive: false,
    isHambuggerActive: false
  }),

  methods: {
    clickToggleBtn() {
      this.$data.isMenuActive = !this.$data.isMenuActive
      this.$data.isIconsActive = !this.$data.isIconsActive
      this.$data.isHambuggerActive = !this.$data.isHambuggerActive
    }
  }
}
</script>

<style scoped>
.navbar {
  --text-color: #f0f4f5;
  --background-color: #263343;
  --accent-color: #d49466;

  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: var(--background-color);
  padding: 8px 12px;
  margin-bottom: 20px;
}

img {
  display: inline;
}

a {
  text-decoration: none;
  color: var(--text-color);
}

.title {
  vertical-align: middle;
}

.navbar__logo {
  font-size: 24px;
  color: var(--text-color);
}

.navbar__logo i {
  color: var(--accent-color);
}

.navbar__logo .logo {
  vertical-align: text-top;
}

.navbar__logo .title {
  vertical-align: text-bottom;
}

.navbar__menu {
  display: flex;
  visibility: hidden;
  list-style: none;
}

.navbar__menu li {
  padding: 8px 12px;
}

.navbar__icons {
  list-style: none;
  color: var(--text-color);
  display: flex;
}

.navbar__icons li {
  padding: 8px 12px;
}

.navbar__menu li:hover {
  background-color: var(--accent-color);
  border-radius: 4px;
}

.navbar__toggleBtn {
  display: none;
  position: absolute;
  right: 32px;
  font-size: 24px;
  color: var(--accent-color);
}

@media screen and (max-width: 768px) {
  .navbar {
    flex-direction: column;
    align-items: flex-start;
    padding: 8px 24px;
  }

  .navbar__menu,
  .navbar__icons {
    display: flex;
    visibility: hidden;
    width: 100%;
    height: 0;
    transition: all 0.3s;
  }

  .navbar__menu {
    flex-direction: column;
    align-items: center;
  }

  .navbar__menu li {
    width: 100%;
    text-align: center;
  }

  .navbar__icons {
    justify-content: center;
  }

  .navbar__menu.active,
  .navbar__icons.active {
    visibility: visible;
    height: fit-content;
  }

  .navbar__toggleBtn {
    display: block;
    transform: rotate(0);
    transition: transform 0.5s;
  }

  .navbar__toggleBtn.active {
    transform: rotate(90deg);
    transition: transform 0.5s;
  }
}
</style>
