<template>
  <div class="context-menu" ref="contextMenu">
    <div
      class="menu"
      tabindex="-1"
      ref="menu"
      v-if="showMenu"
      @blur="closeMenu"
      :style="{ top: top, left: left }"
      @click="closeMenu"
    >
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "ContextMenu",
  data() {
    return {
      showMenu: false,
      top: "0px",
      left: "0px",
    };
  },
  methods: {
    setMenu(top, left) {
      let largestHeight =
        window.innerHeight - this.$refs.menu.offsetHeight - 25;
      let largestWidth = window.innerWidth - this.$refs.menu.offsetWidth - 25;
      if (top > largestHeight) top = largestHeight;
      if (left > largestWidth) left = largestWidth;
      this.top = top + "px";
      this.left = left + "px";
    },

    closeMenu() {
      this.showMenu = false;
      this.$parent.closeMenu();
    },

    openMenu(e) {
      this.showMenu = true;
      this.$nextTick(
        function () {
          this.$refs.menu.focus();
          this.setMenu(e.y, e.x);
        }.bind(this)
      );
      e.preventDefault();
    },
  },
};
</script>

<style lang="scss" scoped>
.context-menu {
  width: 100%;
  height: 100%;
}

.menu {
  position: fixed;
  min-width: 136px;
  max-width: 240px;
  list-style: none;
  background: rgba(255, 255, 255, 0.88);
  box-shadow: 0 6px 12px -4px rgba(0, 0, 0, 0.08);
  border: 1px solid rgba(0, 0, 0, 0.06);
  backdrop-filter: blur(12px);
  border-radius: 8px;
  box-sizing: border-box;
  padding: 6px;
  z-index: 1000;

  &:focus {
    outline: none;
  }
}

[data-theme="dark"] {
  .menu {
    background: rgba(46, 46, 46, 0.68);
    backdrop-filter: blur(16px) contrast(120%);
    border: 1px solid rgba(255, 255, 255, 0.08);
  }
  .menu .item:hover {
    color: var(--color-text);
  }
}

.menu .item {
  font-weight: 600;
  font-size: 14px;
  padding: 10px 14px;
  border-radius: 7px;
  cursor: default;
  color: var(--color-text);
  &:hover {
    color: var(--color-primary);
    background: var(--color-primary-bg-for-transparent);
  }
}

hr {
  margin: 4px 10px;
  background: rgba(128, 128, 128, 0.18);
  height: 1px;
  box-shadow: none;
  border: none;
}

.item-info {
  padding: 10px 10px;
  display: flex;
  align-items: center;
  color: var(--color-text);
  cursor: default;
  img {
    height: 38px;
    width: 38px;
    border-radius: 4px;
  }
  .info {
    margin-left: 8px;
  }
  .title {
    font-size: 16px;
    font-weight: 600;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 1;
    overflow: hidden;
    word-break: break-all;
  }
  .subtitle {
    font-size: 12px;
    opacity: 0.68;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 1;
    overflow: hidden;
    word-break: break-all;
  }
}
</style>
