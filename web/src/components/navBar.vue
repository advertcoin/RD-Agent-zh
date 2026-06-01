<template>
  <div class="header-component">
    <div class="nav">
      <router-link to="/">
        <div class="logo-container">
          <img src="@/assets/images/RDAgent-logo.png" alt="R&D-Agent logo" />
        </div>
      </router-link>
      <ul>
        <li>
          <router-link to="/">{{ t('nav.home') }}</router-link>
        </li>
        <li>
          <router-link to="/Playground">{{ t('nav.playground') }}</router-link>
        </li>
      </ul>
      <div class="lang-switch" @click="toggleLang">
        {{ currentLocale === 'zh' ? 'EN' : '中文' }}
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { useI18n } from '../i18n';
export default {
  components: {},
  setup() {
    const { t, setLocale, currentLocale } = useI18n();
    
    const toggleLang = () => {
      const newLang = currentLocale.value === 'zh' ? 'en' : 'zh';
      setLocale(newLang);
      window.location.reload();
    };
    
    return {
      t,
      currentLocale,
      toggleLang
    };
  },
};
</script>
<style scoped lang="scss">
.header-component {
  padding: 1.75em 4.375em 0;
  // height: 6.125em;
  height: 5.15em;
  box-sizing: border-box;
  & > .nav {
    display: flex;
    flex-wrap: nowrap;
    justify-content: flex-start;
    align-items: center;
    flex-direction: row;
    .logo-container {
      margin-right: 4.375em;
      img {
        display: inline-block;
        height: 2.25em;
      }
    }
    ul {
      display: flex;
      flex-direction: row;
      li {
        margin-right: 2.25em;
        text-align: center;
        cursor: pointer;
        a {
          display: inline-block;
          color: var(--nav-default-color);
          font-size: 1.25em;
          font-weight: 700;
          line-height: 200%;
          &:hover {
            color: var(--nav-hover-color);
          }
        }
        .router-link-exact-active {
          color: var(--text-color);

          &:hover {
            color: var(--text-color);
          }
        }
      }
    }
    .lang-switch {
      margin-left: auto;
      padding: 0.5em 1em;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      border-radius: 999px;
      color: #fff;
      font-size: 1em;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
      
      &:hover {
        transform: scale(1.05);
        box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
      }
    }
  }
}
</style>
