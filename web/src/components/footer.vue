<template>
  <footer>
    <div class="footer" :style="{ background: color }">
      <p>
        <a target="_blank" aria-label="Contact us" href="#">{{ t('footer.contact') }}</a>
        <span class="line">|</span>
        <a
          target="_blank"
          aria-label="Privacy & Cookies"
          href="https://go.microsoft.com/fwlink/?LinkId=521839"
          >{{ t('footer.privacy') }}</a
        >
        <span class="line">|</span>
        <a
          target="_blank"
          aria-label="Terms of Use"
          href="https://go.microsoft.com/fwlink/?LinkID=206977"
          >{{ t('footer.terms') }}</a
        >
        <span class="line">|</span>
        <a
          target="_blank"
          aria-label="Trademarks"
          href="https://www.microsoft.com/trademarks"
          >{{ t('footer.trademarks') }}</a
        >
        <span class="line">|</span>
        <span style="">{{ t('footer.copyright') }}</span>
        <span class="line">|</span>
        <span>{{ t('footer.disclaimer') }}</span>
      </p>
    </div>
  </footer>
</template>
<script>
import { useI18n } from '../i18n';
export default {
  components: {},
  props: {
    color: {
      type: String,
      required: true,
    },
  },
  setup() {
    const { t } = useI18n();
    return { t };
  },
};
</script>
<style scoped lang="scss">
.footer {
  padding: 1.75em 4.375em;
  box-sizing: border-box;
  text-align: center;
  width: 100%;
  color: var(--footer-text-color);
  // background: var(--bg-grey);
  // background: var(--bg-white);
  font-size: 12px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  flex-direction: row;
  position: relative;
  a {
    color: var(--footer-text-color);
    font-weight: 400;
    font-size: 12px;
    line-height: 21px;
    text-decoration: none;
    display: inline-block;
  }
  .line {
    margin: 0 16px;
    display: inline-block;
    vertical-align: top;
  }
  p {
    display: flex;
    align-items: center;
    margin: 0;

    &:nth-child(2) a {
      display: inline-block;
      img {
        height: 1.375em;
        vertical-align: middle;
      }
    }
  }
}
</style>
