<template>
  <div class="hover" id="hoverLayer" v-if="isShow">
    <div class="reveal">hello，啵啵!</div>
    <div class="btnWr">
      <button data-text="进入" class="btn btn-primary btn-ghost btn-border-stroke btn-text-float-up" @click="hoverBtnClik">
        <div class="btn-borders">
          <div class="border-top"></div>
          <div class="border-right"></div>
          <div class="border-bottom"></div>
          <div class="border-left"></div>
        </div>
        <span class="btn-text">开始</span>
      </button>
    </div>
  </div>
  <div v-else>
    <div>
      <header class="page-header">
        <h1 class="title slide-bar">你好啊，许牧楠！很高兴认识你</h1>
        <p class="subtitle slide-bar">一个人的记忆叫做回忆，而两个人的回忆就叫做爱情。</p>
      </header>
    </div>

    <div class="love-wrap">
      <CustomerImg v-for="(item, index) in imgArr" :key="index" :brief="item.brief" :src="item.src" @clickFn="dialogShow(index)" />
      <dialog id="confirm-modal" class="modal" ref="dialogRef">
        <!-- <div class="modal-content"> -->
          <!-- <h2 class="modal-title"></h2> -->
          <!-- <p class="modal-description">{{imgArr[clickedIndex].text}}</p> -->
          <img :src="imgArr[clickedIndex].src" />
          <!-- <div class="modal-options">
            <button
              class="btn btn-round btn-fill btn-fill-left option confirm"
              data-text="Yes"
              onclick="document.querySelector('#confirm-modal').close()"
            ></button>
            <button
              class="btn btn-round btn-fill btn-fill-right option cancel"
              data-text="No"
              onclick="document.querySelector('#confirm-modal').close()"
            ></button>
          </div> -->
        <!-- </div> -->
      </dialog>
    </div>
  </div>
</template>

<script>
import CustomerImg from "./components/CustomerImg.vue";
import { ref, onMounted } from "vue";
import imgArr from "./config";
export default {
  name: "App",
  components: {
    CustomerImg,
  },
  setup() {
    console.log("imgArr: ", imgArr);
    const dialogRef = ref(null);

    const isShow = ref(true);

    const clickedIndex = ref(0)

    const bodyClick = (e) => {
      if (dialogRef.value && dialogRef.value.open && e.target.id === "confirm-modal") {
        dialogRef.value.close();
      }
    };

    onMounted(() => {
      document.body.addEventListener("click", bodyClick, true);

      let duration = 0.8;
      let delay = 0.3;
      let revealText = document.querySelector(".reveal");
      let letters = revealText.textContent.split("");
      revealText.textContent = "";
      let middle = letters.filter((e) => e !== " ").length / 2;
      letters.forEach((letter, i) => {
        let span = document.createElement("span");
        span.textContent = letter;
        span.style.animationDelay = `${delay + Math.abs(i - middle) * 0.1}s`;
        revealText.append(span);
      });
    });

    onMounted(() => {
      document.body.addEventListener("click", bodyClick);
    });

    return {
      imgArr,
      dialogRef,
      isShow,
      clickedIndex,
      dialogShow: (index) => {
        console.log("index: ", index);
        dialogRef.value.showModal();
        clickedIndex.value = index;
      },
      dialogHide: () => {
        dialogRef.value.close();
      },
      hoverBtnClik: (e) => {
        isShow.value = false;
        // document.body.requestFullscreen();
      },
    };
  },
};
</script>

<style lang="less">
body {
  height: 100vh;
  width: 100vw;
  background: #1a1e23;
  overflow: hidden;
}

.hover {
  position: absolute;
  top: 0;
  right: 0;
  z-index: 1000;
  height: 100vh;
  width: 100vw;
  background: black;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  transition: all 0.5 ease;

  .btnWr {
    width: 100%;
  }
}

#app {
  display: flex;
  flex-direction: column;
}

.love-wrap {
  height: 520px;
  width: 1314px;
  display: flex;
  flex-flow: row wrap;
  margin: 0 auto;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 999;
  color: white;
  background-image: linear-gradient(to right, #0acffe 0%, #495aff 100%);
  border: transparent;
  border-radius: 12px;
  box-shadow: 0 2.8px 2.2px rgba(0, 0, 0, 0.02), 0 6.7px 5.3px rgba(0, 0, 0, 0.028), 0 12.5px 10px rgba(0, 0, 0, 0.035),
    0 22.3px 17.9px rgba(0, 0, 0, 0.042), 0 41.8px 33.4px rgba(0, 0, 0, 0.05), 0 100px 80px rgba(0, 0, 0, 0.07);
  animation: show-modal 0.5s ease forwards;

  &::backdrop {
    background: rgba(0, 0, 0, 0.4);
    backdrop-filter: blur(3px);
  }

  .model-icon {
    margin-bottom: 1.25rem;
    opacity: 0;
    animation: show-modal-icon 0.5s ease 0.2s forwards;
  }

  .modal-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 300px;
    padding: 1em;

    .modal-title {
      margin-top: 0;
      margin-bottom: 1.2rem;
      opacity: 0;
      animation: show-modal-text 0.5s ease 0.35s forwards;
    }

    .modal-description {
      margin: 0;
      opacity: 0;
      animation: show-modal-text 1s ease 0.5s forwards;
    }

    .modal-options {
      margin-top: 1rem;
      display: flex;
      justify-content: space-around;

      .option {
        padding: 0 2em;
        margin: 0.3em;
        font-size: 20px;
        font-weight: 700;
        line-height: 2;
      }

      .confirm {
        opacity: 0;
        animation: show-modal-option 0.5s ease 0.65s forwards;

        &::before {
          background: hsl(141, 53%, 53%);
        }

        &::after {
          color: hsl(141, 53%, 53%);
        }
      }

      .cancel {
        opacity: 0;
        animation: show-modal-option 0.5s ease 0.8s forwards;

        &::before {
          background: hsl(348, 86%, 61%);
        }

        &::after {
          color: hsl(348, 86%, 61%);
        }
      }
    }
  }
}

@keyframes show-modal {
  from {
    transform: scale(0.8);
  }

  50% {
    transform: scale(1.1);
    opacity: 1;
  }

  to {
    transform: scale(1);
    opacity: 1;
  }
}

@keyframes show-modal-icon {
  from {
    transform: scale(0.4);
  }

  50% {
    transform: scale(1.2);
    opacity: 1;
  }

  to {
    transform: scale(1);
    opacity: 1;
  }
}

@keyframes show-modal-text {
  from {
    transform: scale(0.6);
  }

  50% {
    transform: scale(1.2);
    opacity: 1;
  }

  to {
    transform: scale(1);
    opacity: 1;
  }
}

@keyframes show-modal-option {
  from {
    transform: scale(0.4);
  }

  50% {
    transform: scale(1.2);
    opacity: 1;
  }

  to {
    transform: scale(1);
    opacity: 1;
  }
}

.reveal {
  position: relative;
  display: flex;
  color: #6ee1f5;
  font-size: 2em;
  font-family: Raleway, sans-serif;
  letter-spacing: 3px;
  text-transform: uppercase;
  white-space: pre;

  span {
    opacity: 0;
    transform: scale(0);
    animation: fadeIn 2.4s forwards;
  }

  &::before,
  &::after {
    position: absolute;
    content: "";
    top: 0;
    bottom: 0;
    width: 2px;
    height: 100%;
    background: white;
    opacity: 0;
    transform: scale(0);
  }

  &::before {
    left: 50%;
    animation: slideLeft 1.5s cubic-bezier(0.7, -0.6, 0.3, 1.5) forwards;
  }

  &::after {
    right: 50%;
    animation: slideRight 1.5s cubic-bezier(0.7, -0.6, 0.3, 1.5) forwards;
  }
}

@keyframes fadeIn {
  to {
    opacity: 1;
    transform: scale(1);
  }
}

@keyframes slideLeft {
  to {
    left: -6%;
    opacity: 1;
    transform: scale(0.9);
  }
}

@keyframes slideRight {
  to {
    right: -6%;
    opacity: 1;
    transform: scale(0.9);
  }
}

.btn {
  --hue: 190;
  --ease-in-duration: 0.25s;
  --ease-in-exponential: cubic-bezier(0.95, 0.05, 0.795, 0.035);
  --ease-out-duration: 0.65s;
  --ease-out-delay: var(--ease-in-duration);
  --ease-out-exponential: cubic-bezier(0.19, 1, 0.22, 1);
  position: relative;
  padding: 1rem 3rem;
  font-size: 1rem;
  line-height: 1.5;
  color: white;
  text-decoration: none;
  background-color: hsl(var(--hue), 100%, 41%);
  border: 1px solid hsl(var(--hue), 100%, 41%);
  outline: transparent;
  overflow: hidden;
  cursor: pointer;
  user-select: none;
  white-space: nowrap;
  transition: 0.25s;

  &:hover {
    background: hsl(var(--hue), 100%, 31%);
  }

  &-primary {
    --hue: 171;
  }

  &-ghost {
    color: hsl(var(--hue), 100%, 41%);
    background-color: transparent;
    border-color: hsl(var(--hue), 100%, 41%);

    &:hover {
      color: white;
    }
  }

  &-border-stroke {
    border-color: hsla(var(--hue), 100%, 41%, 0.35);

    .btn-borders {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;

      .border-top {
        position: absolute;
        top: 0;
        width: 100%;
        height: 1px;
        background: hsl(var(--hue), 100%, 41%);
        transform: scaleX(0);
        transform-origin: left;
      }

      .border-right {
        position: absolute;
        right: 0;
        width: 1px;
        height: 100%;
        background: hsl(var(--hue), 100%, 41%);
        transform: scaleY(0);
        transform-origin: bottom;
      }

      .border-bottom {
        position: absolute;
        bottom: 0;
        width: 100%;
        height: 1px;
        background: hsl(var(--hue), 100%, 41%);
        transform: scaleX(0);
        transform-origin: left;
      }

      .border-left {
        position: absolute;
        left: 0;
        width: 1px;
        height: 100%;
        background: hsl(var(--hue), 100%, 41%);
        transform: scaleY(0);
        transform-origin: bottom;
      }

      // when unhover, ease-out left, bottom; ease-in right, top

      .border-left {
        transition: var(--ease-out-duration) var(--ease-out-delay) var(--ease-out-exponential);
      }

      .border-bottom {
        transition: var(--ease-out-duration) var(--ease-out-delay) var(--ease-out-exponential);
      }

      .border-right {
        transition: var(--ease-in-duration) var(--ease-in-exponential);
      }

      .border-top {
        transition: var(--ease-in-duration) var(--ease-in-exponential);
      }
    }

    &:hover {
      color: hsl(var(--hue), 100%, 41%);
      background: transparent;

      .border-top,
      .border-bottom {
        transform: scaleX(1);
      }

      .border-left,
      .border-right {
        transform: scaleY(1);
      }

      // when hover, ease-in left, bottom; ease-out right, top

      .border-left {
        transition: var(--ease-in-duration) var(--ease-in-exponential);
      }

      .border-bottom {
        transition: var(--ease-in-duration) var(--ease-in-exponential);
      }

      .border-right {
        transition: var(--ease-out-duration) var(--ease-out-delay) var(--ease-out-exponential);
      }

      .border-top {
        transition: var(--ease-out-duration) var(--ease-out-delay) var(--ease-out-exponential);
      }
    }
  }

  &-text-float-up {
    &::after {
      position: absolute;
      content: attr(data-text);
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      opacity: 0;
      transform: translateY(35%);
      transition: 0.25s ease-out;
    }

    // when hover, ease-in top-text; ease-out bottom-text

    .btn-text {
      display: block;
      transition: 0.75s 0.1s var(--ease-out-exponential);
    }

    &:hover {
      // when hover, ease-in bottom-text; ease-out top-text

      .btn-text {
        opacity: 0;
        transform: translateY(-25%);
        transition: 0.25s ease-out;
      }

      &::after {
        opacity: 1;
        transform: translateY(0);
        transition: 0.75s 0.1s var(--ease-out-exponential);
      }
    }
  }
}

.page-header {
  display: inline-block;

  .slide-bar {
    position: relative;
    color: transparent;
    animation: fill-text-white 2s 1.6s forwards;

    &::before {
      position: absolute;
      content: "";
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #35b9f1;
      transform: scaleX(0);
      transform-origin: left;
      animation: slide-in-out 2s cubic-bezier(0.75, 0, 0, 1) forwards;
    }
  }

  @keyframes slide-in-out {
    50% {
      transform: scaleX(1);
      transform-origin: left;
    }

    50.1% {
      transform-origin: right;
    }

    100% {
      transform: scaleX(0);
      transform-origin: right;
    }
  }

  @keyframes fill-text-white {
    to {
      color: white;
    }
  }

  header {
    .title,
    .subtitle {
      width: 250px;
      height: 30px;
    }

    .title {
      margin: 0;
      font-family: Lora, serif;
      font-size: 32px;
      line-height: 30px;

      &::before {
        background: #ff4081;
      }
    }

    .subtitle {
      margin: 10px 0 0 0;
      font-family: Lato, sans-serif;
      font-size: 12px;
      line-height: 30px;
      letter-spacing: 5px;
      text-transform: uppercase;
      animation-delay: 3.2s;

      &::before {
        background: #03a9f4;
        animation-delay: 2s;
      }
    }
  }
}
</style>
