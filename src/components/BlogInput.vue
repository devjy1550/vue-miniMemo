<template>
  <div class="input-wrap shadow">
    <input
      type="text"
      v-model="newItem"
      class="input-box"
      maxlength="30"
      @keyup.enter="addItem"
    />

    <div class="option">
      <div class="img-list">
        <span @click="addIcon(0)" class="img1 img-active"></span>
        <span @click="addIcon(1)" class="img2"></span>
        <span @click="addIcon(2)" class="img3"></span>
      </div>

      <span @click="addItem" class="add-bt">
        <i class="fas fa-plus add-bt-icon"></i>
      </span>
    </div>
    <!-- 안내창 -->
    <AlertView v-bind:show="showModal" v-on:closemodal="showModal = false">
      <template #header>
        <h3>-입력 요망-</h3>
      </template>
      <template #body>
        <h2>내용을 작성하시오.</h2>
      </template>
    </AlertView>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

import { useStore } from "vuex";

import AlertView from "@/components/common/AlertVue.vue";

export default {
  components: {
    AlertView,
  },

  setup() {
    let imgs;
    onMounted(() => {
      imgs = document.querySelectorAll(".img-list > span");
      console.log(imgs);
    });

    const store = useStore();

    const newItem = ref("");
    const newIcon = ref(0);
    const showModal = ref(false);

    const addItem = () => {
      let temp = newItem.value;
      let icon = newIcon.value;
      // 앞쪽 뒷쪽 공백 제거
      temp = temp.trim();
      // 추후 업데이트 예정(정규표현식-문자열체크 문법)
      //  앞자리공백   공백    뒷자리공백
      if (temp !== "") {
        // context.emit("additem", temp, icon);
        // store.commit('ADD_MEMO' {item:temp, index:icon});
        store.dispatch("fetchAddMemo", {
          item: temp,
          index: icon,
        });

        resetItem();
      } else {
        showModal.value = true;
      }
    };

    // 내용 재설정
    const resetItem = () => {
      newItem.value = "";
      addIcon(0);
    };

    const addIcon = (index) => {
      newIcon.value = index;
      for (let i = 0; i < imgs.length; i++) {
        imgs[i].classList.remove("img-active");
      }
      imgs[index].classList.add("img-active");
    };

    return {
      newItem,
      addItem,
      addIcon,
      showModal,
    };
  },
};
</script>

<style scoped>
.input-wrap {
  position: relative;
  display: block;
  line-height: 50px;
  border-radius: 5px;
  background-color: rgb(255, 253, 234);

  overflow: hidden;
  margin-top: 20px;
}

.input-wrap input {
  border-style: none;
}

.input-wrap input:focus {
  outline: none;
}

.input-box {
  width: calc(100% - 240px);
  font-size: 16px;
  margin-left: 20px;
  border-radius: 2px;
  background-color: rgb(255, 255, 255);
}

.option {
  position: absolute;
  display: block;
  right: 0;
  top: 0;
  width: 220px;
  height: 50px;
  font-size: 0;
}

.img-list {
  display: inline-block;
}

.img1 {
  position: relative;
  display: inline-block;
  width: 50px;
  height: 50px;
  line-height: 50px;
  font-size: 0;
  cursor: pointer;
  background: url("@/assets/images/first.png") no-repeat center;
  opacity: 0.3;
  background-size: 30px 15px;
  margin-left: 5px;
}

.img2 {
  position: relative;
  display: inline-block;
  width: 50px;
  height: 50px;
  line-height: 50px;
  font-size: 0;
  cursor: pointer;
  background: url("@/assets/images/second.png") no-repeat center;
  opacity: 0.3;
  background-size: 30px 15px;
  margin-left: 5px;
}

.img3 {
  position: relative;
  display: inline-block;
  width: 50px;
  height: 50px;
  line-height: 50px;
  font-size: 0;
  cursor: pointer;
  background: url("@/assets/images/third.png") no-repeat center;
  opacity: 0.3;
  background-size: 30px 15px;
  margin-left: 5px;
}
.img-active {
  opacity: 1;
}
.img-list > span:hover {
  opacity: 1;
}

.add-bt {
  position: relative;
  display: inline-block;
  vertical-align: top;
  width: 50px;
  background-color: rgb(255, 216, 44);
  cursor: pointer;
  margin-left: 5px;
}
.add-bt:hover {
  background-color: rgb(243, 203, 24);
}

.add-bt-icon {
  display: inline-block;
  color: #fff;
  font-size: 10px;
  width: 50px;
  text-align: center;
  vertical-align: middle;

  /* font-size: 20px; */
}
</style>
