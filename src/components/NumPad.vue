<script setup lang="ts">
import { onUpdated, ref, watch } from "vue";
import { ArrowLeftBold } from "@element-plus/icons-vue";
import { defineEmits, defineProps } from "vue";

const currentAmount = ref("0");

const emit = defineEmits<{
  (e: "currentAmountChange", value: string): void;
}>();

const props = defineProps<{
  initAmount: string;
}>();

// This component should output an xmr value.  Could be tied directly to store or emit to parent

onUpdated(() => {
  if (props.initAmount === "0") {
    currentAmount.value = "0";
  }
});
watch(currentAmount, (newAmount) => {
  if (currentAmount.value === "") {
    currentAmount.value += "0";
  }

  emit("currentAmountChange", newAmount);
});

const updateAmount = (val: string) => {
  if (currentAmount.value === "0") {
    currentAmount.value = val;
  } else if (val === ".") {
    if (currentAmount.value.indexOf(".") === -1) {
      currentAmount.value += ".";
    }
  } else {
    currentAmount.value += val;
  }
};
</script>
<template>
  <div class="wrapper">
    <el-row justify="center">
      <el-space>
        <el-button text class="numpad" @click="updateAmount('1')">1</el-button>
        <el-button text class="numpad" @click="updateAmount('2')">2</el-button>
        <el-button text class="numpad" @click="updateAmount('3')">3</el-button>
      </el-space>
      <el-space>
        <el-button text class="numpad" @click="updateAmount('4')">4</el-button>
        <el-button text class="numpad" @click="updateAmount('5')">5</el-button>
        <el-button text class="numpad" @click="updateAmount('6')">6</el-button>
      </el-space>
      <el-space>
        <el-button text class="numpad" @click="updateAmount('7')">7</el-button>
        <el-button text class="numpad" @click="updateAmount('8')">8</el-button>
        <el-button text class="numpad" @click="updateAmount('9')">9</el-button>
      </el-space>
      <el-space>
        <el-button
          text
          class="numpad"
          @click="
            currentAmount.indexOf('.') === -1 ? (currentAmount += '.') : ''
          "
          >.
        </el-button>
        <el-button text class="numpad" @click="updateAmount('0')">0</el-button>
        <el-button
          text
          class="numpad"
          @click="
            currentAmount = currentAmount.slice(0, currentAmount.length - 1)
          "
        >
          <el-icon :size="20">
            <ArrowLeftBold />
          </el-icon>
        </el-button>
      </el-space>
    </el-row>
  </div>
</template>
<style scoped>
.wrapper {
  max-width: 400px;
  margin-bottom: 20px;
}

.currency-select {
  width: 75px;
  /* TODO: add border radius here */
  border-radius: 20px;
}

button {
  /* TODO: size buttons/fonts with e+ breakpoints */
  border: 1px solid gray;
  min-width: 100px;
  padding: 30px 20px;
  font-size: 35px;
  touch-action: manipulation;
}
</style>
