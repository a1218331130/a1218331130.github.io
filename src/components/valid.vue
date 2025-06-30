<template>
  <div>
    <van-radio-group
      v-model="checked"
      direction="horizontal"
      style="font-size: 0.35rem; margin: 0 0 0.5rem 0.5rem"
      @change="radioChange"
    >
      <van-radio name="1">普通车牌</van-radio>
      <van-radio name="2">新能源车牌</van-radio>
    </van-radio-group>

    <!-- 车牌输入框： 京A·123456A-->
    <!--正常车牌 1、车牌号省的简称；2、各地级市字母代码；3、5位[数字+字母]。 -->
    <!--新能源车牌 1、车牌号省的简称；2、各地级市字母代码；3、6位[数字+字母]。 -->
    <div class="worldInput">
      <ul class="van-password-input__security van-hairline--surround">
        <li v-for="(item, index) in inputValue" :key="item">
          <div
            :class="`lis ${
              inputValue.length > 1 && index === 1 ? 'li_dian' : ''
            }`"
          >
            {{ item }}
          </div>
        </li>
        <!-- 占位  -->
        <li v-for="(item1, index) in liNumber" :key="item1">
          <div
            :class="`lis ${
              (index === 1 && inputValue.length === 0) ||
              (index === 0 && inputValue.length === 1)
                ? 'li_dian'
                : ''
            }`"
          ></div>
        </li>
      </ul>
    </div>
    <!--键盘-->
    <div class="keyboard">
      <div class="keyboardLayout">
        <div class="keyboardLayout_left">
          <!-- 中文 -->
          <van-button
            native-type="button"
            v-show="isShowKeyboard"
            size="small"
            v-for="(item, index) in chinesKeyBoard"
            :key="item.id"
            @click="clickChinesKeyBoard(index)"
            class="keyboardButton"
          >
            {{ item.name }}
          </van-button>
          <!-- 英文 -->
          <van-button
            v-for="(item, index) in englishKeyBoard"
            :disabled="item.disabled"
            native-type="button"
            v-show="!isShowKeyboard"
            size="small"
            :key="item.id"
            @click="clickEnglishKeyBoard(index)"
            class="keyboardButton_english"
          >
            {{ item.name }}
          </van-button>
        </div>
        <!-- 刪除键&切换键&完成键 -->
        <div class="keyboardLayout_right">
          <div class="layout_right">
            <template
              v-for="(item, index) in keyboardLayoutRight"
              :key="item.id"
            >
              <van-button
                native-type="button"
                size="small"
                @click="clickKeyboardLayoutRight(index)"
                class="layout_right_button"
                v-if="item.id !== 3"
              >
                {{ item.name }}
              </van-button>
              <van-button
                native-type="button"
                size="small"
                :disabled="isShowKeyboard"
                @click="clickKeyboardLayoutRight(isShowKeyboard)"
                class="layout_right_button"
                v-if="item.id === 3"
              >
                {{ `${isShowKeyboard ? `ABC` : `省份`}` }}
              </van-button>
            </template>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, defineEmits, onMounted } from "vue";
import { showToast } from "vant";

const emit = defineEmits(["defineEmits"]);

// 输入框
const inputValue = ref(["", "", "·"]);

// 输入框个数 -默认普通车牌号个数
const liNumber = ref(5);

const initLiNumber = ref(8);
// 车牌类型
const checked = ref("1");
const radioChange = () => {
  if (checked.value === "1") {
    liNumber.value = 5;
    initLiNumber.value = 8;
    inputValue.value = ["", "", "·"];
    isShowKeyboard.value = true;
  } else {
    liNumber.value = 6;
    initLiNumber.value = 9;
    inputValue.value = ["", "", "·"];
    isShowKeyboard.value = true;
  }
};

// 中英文键盘切换
const isShowKeyboard = ref(true);

// 中文
const chinesKeyBoard = ref([
  { name: "京", id: 1 },
  { name: "津", id: 2 },
  { name: "冀", id: 3 },
  { name: "晋", id: 4 },
  { name: "蒙", id: 5 },
  { name: "辽", id: 6 },
  { name: "吉", id: 7 },
  { name: "黑", id: 8 },
  { name: "沪", id: 9 },
  { name: "苏", id: 10 },
  { name: "浙", id: 11 },
  { name: "皖", id: 12 },
  { name: "闽", id: 13 },
  { name: "赣", id: 14 },
  { name: "鲁", id: 15 },
  { name: "豫", id: 16 },
  { name: "鄂", id: 17 },
  { name: "湘", id: 18 },
  { name: "粤", id: 19 },
  { name: "桂", id: 20 },
  { name: "琼", id: 21 },
  { name: "渝", id: 22 },
  { name: "川", id: 23 },
  { name: "贵", id: 24 },
  { name: "云", id: 25 },
  { name: "藏", id: 26 },
  { name: "陕", id: 27 },
  { name: "甘", id: 28 },
  { name: "青", id: 29 },
  { name: "宁", id: 30 },
  { name: "新", id: 31 },
  { name: "台", id: 32 },
  { name: "港", id: 33 },
  { name: "澳", id: 34 },

  // { name: '·', id: 32 },
  // { name: '英', id: 99 },
]);
const clickChinesKeyBoard = (chineseValue) => {
  if (inputValue.value.length < liNumber.value) {
    inputValue.value.splice(0, 1, chinesKeyBoard.value[chineseValue].name);
    if (liNumber.value !== 0 && inputValue.value.length > 3) {
      liNumber.value--;
    }
  }
  if (inputValue.value.length >= 1) {
    isShowKeyboard.value = false;
    englishKeyBoard.value.map((mapObj) => (mapObj.disabled = mapObj.id < 38));
  }
};
// 英文
const englishKeyBoard = ref([
  { name: "0", id: 37 },
  { name: "1", id: 28 },
  { name: "2", id: 29 },
  { name: "3", id: 30 },
  { name: "4", id: 31 },
  { name: "5", id: 32 },
  { name: "6", id: 33 },
  { name: "7", id: 34 },
  { name: "8", id: 35 },
  { name: "9", id: 36 },
  { name: "Q", id: 38 },
  { name: "W", id: 39 },
  { name: "E", id: 40 },
  { name: "R", id: 41 },
  { name: "T", id: 42 },
  { name: "Y", id: 43 },
  { name: "U", id: 44 },
  { name: "I", id: 45 },
  { name: "O", id: 46 },
  { name: "P", id: 47 },
  { name: "A", id: 48 },
  { name: "S", id: 49 },
  { name: "D", id: 50 },
  { name: "F", id: 51 },
  { name: "G", id: 52 },
  { name: "H", id: 53 },
  { name: "J", id: 54 },
  { name: "K", id: 55 },
  { name: "L", id: 56 },
  { name: "Z", id: 57 },
  { name: "X", id: 58 },
  { name: "C", id: 59 },
  { name: "V", id: 60 },
  { name: "B", id: 61 },
  { name: "N", id: 62 },
  { name: "M", id: 63 },
  // { name: '中', id: 99 },
]);
const clickEnglishKeyBoard = (englishValue) => {
  if (inputValue.value.length < initLiNumber.value) {
    if (inputValue.value.length === 3 && inputValue.value[1] === "") {
      inputValue.value.splice(1, 1, englishKeyBoard.value[englishValue].name);
    } else {
      inputValue.value.push(englishKeyBoard.value[englishValue].name);
    }
    if (liNumber.value !== 0 && inputValue.value.length > 3) {
      liNumber.value--;
    }
    if (inputValue.value.length > 2) {
      englishKeyBoard.value.map((mapObj) => (mapObj.disabled = false));
    }
  }
};

// 键盘-非中英文
const keyboardLayoutRight = ref([
  { name: "←", id: 1 },
  { name: `ABC`, id: 3 },
  { name: "完成", id: 2 },
]);

// 车牌正则校验
function isVehicleNumber(vehicleNumber) {
  let result = false;
  // 车牌7位+·  一共8位
  if (vehicleNumber.length === initLiNumber.value) {
    const expressArr = [
      /^[京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领台港澳A-Z]{1}[A-Z]{1}[·]{1}[A-Z0-9]{4}[A-Z0-9挂学警港澳]{1}$/,
      /^[京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领台港澳A-Z]{1}[A-Z]{1}[·]{1}(([0-9]{5}[DABCEFGHJK]$)|([DABCEFGHJK][A-HJ-NP-Z0-9][0-9]{4}$))/,
    ];
    // 普通车牌规则
    // let express =
    //   /^[京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领A-Z]{1}[A-Z]{1}[·]{1}[A-Z0-9]{4}[A-Z0-9挂学警港澳]{1}$/;
    // 新能源车牌规则
    // let express1 =
    //   /^[京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领A-Z]{1}[A-Z]{1}[·]{1}(([0-9]{5}[DABCEFGHJK]$)|([DABCEFGHJK][A-HJ-NP-Z0-9][0-9]{4}$))/; // 2021年新能源车牌不止有DF

    result = expressArr[Number(checked.value - 1)].test(vehicleNumber);
  }
  return result;
}

const clickKeyboardLayoutRight = (rightValue) => {
  let rightValueId = keyboardLayoutRight.value[rightValue]?.id;
  // 删除-从结尾删除
  if (rightValueId === 1) {
    if (inputValue.value.length === 3) {
      if (inputValue.value[0] === "") {
        showToast("已清空！");
      } else if (inputValue.value[1] === "") {
        inputValue.value.splice(0, 1, "");
        isShowKeyboard.value = true;
      } else {
        inputValue.value.splice(1, 1, "");
        englishKeyBoard.value.map(
          (mapObj) => (mapObj.disabled = mapObj.id < 38)
        );
      }
    } else {
      inputValue.value.pop();
      if (liNumber.value < initLiNumber.value) {
        liNumber.value += 1;
      }
    }
  }
  // 完成-车牌格式正确则上传-不正确则提示重新输入
  if (rightValueId === 2) {
    let licensePlate = inputValue.value.join("");
    let isVehicleNumberReault = isVehicleNumber(licensePlate);
    isVehicleNumberReault
      ? emit("getLicensePlate", licensePlate)
      : showToast("车牌规则错误，请重新输入！");
  }

  if (typeof rightValue === "boolean") {
    isShowKeyboard.value = !rightValue;
  }
};
onMounted(() => {
  const params = new URLSearchParams({
    access_token: "2b961279f19700d418fb5edd054e7b93",
    page: 1,
    per_page: 100,
  });

  fetch(`https://gitee.com/api/v5/gists?${params}`)
    .then((response) => response.json())
    .then((data) => {
      debugger;
    });
});
</script>

<style>
.worldInput {
  position: relative;
  margin: 16px 0;
  -webkit-user-select: none;
  user-select: none;
}
li {
  border: none;
}
.lis {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  border: 1px solid #f2f3f5;
}
</style>