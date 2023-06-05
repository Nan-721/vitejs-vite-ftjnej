<script setup>
import { useMouse } from '../mouse.js';
import { ref, reactive, onMounted, nextTick, computed, watch } from 'vue';

// 组件参数
defineProps({
  msg: String,
});

onMounted(() => {
  mutateDeeply();
});
console.log('useMouse:>>>>', useMouse);
console.log('useMouse:>>>>()()', useMouse());
const { x, y } = useMouse();
const ref0 = ref(0);
// console.log('ref0==', ref0);
// console.log('ref0==value:>>>', ref0);
ref0.value++;
// console.log('ref0==', ref0.value);

const objectRef = ref({ count: 0 });

// 这是响应式的替换
objectRef.value = { count: 1 };
console.log('objectRef===', objectRef);
console.log('objectRef===value:::>>>', objectRef.value);

// ref() 让我们能创造一种对任意值的 “引用”，并能够在不丢失响应性的前提下传递这些引用。这个功能很重要，因为它经常用于将逻辑提取到 组合函数 中。
const obj = {
  foo: ref(1),
  bar: ref(2),
};

// 该函数接收一个 ref
// 需要通过 .value 取值
// 但它会保持响应性
// callSomeFunction(obj.foo)

// 仍然是响应式的
const { foo, bar } = obj;

const state = reactive({ count1: 0 });

// n是局部变量 这两行不会响应式 state
let n = state.count1;
// 不会影响原始的state
n++;
let { count1 } = state;
// 不会影响原始的state
n++;
// 该函数接收一个普通数字，

function increament() {
  console.log('in==increament', state.count1);
  state.count1++;
  nextTick(() => {
    console.log('访问nextTick');
  });
}
// 深层响应
const obj1 = reactive({
  nested: { count: 0 },
  arr: ['foo', 'bar'],
});
function mutateDeeply() {
  obj1.nested.count++;
  obj1.arr.push('baz');
  console.log('obj1====', JSON.stringify(obj1));
}

const books = reactive([ref('Vue3 Guide')]);
console.log('books===:::>>', books);
console.log('books===:::>>value', books[0].value);

const map = reactive(new Map([['count', ref(0)]]));
console.log('map===:::>>>', map);
console.log('map===:::>>>count', map.get('count'));
console.log('map===:::>>>value', map.get('count').value);

// 计算属性
const author = reactive({
  name: 'John Doe',
  books: ['vue1::::', 'vue2::::', 'vue3::::'],
});
const publishedBookMessage = computed(() => {
  return author.books.length > 0 ? 'yes' : 'no';
});

const firstName = ref('John');
const lastName = ref('Doe');
const fullName = computed({
  get() {
    return firstName.value + ' ' + lastName.value;
  },
  set(newValue) {
    [firstName.value, lastName.value] = newValue.split(' ');
    // [firstName.value, lastName.value] = [newValue, newValue];
  },
});
fullName.value = 'John Doe';

const items = ref([{ message: 'Foo' }, { message: 'Bar' }]);
items.value = items.value.filter((item) => item.message.match(/Foo/));
console.warn('items::>>>???', items.value);
const myObject = reactive({
  title: 'How to do list in Vue',
  author: 'Jane Doe',
  publishedAt: '2016-04-10',
});
const numbers = ref([1, 2, 3, 4, 5, 6]);
const evenNumbers = computed(() => {
  return numbers.value.filter((n) => n % 2 === 0);
});
// 多层嵌套数组循环
const sets = ref([
  [1, 2, 3, 4, 5, 6],
  [7, 8, 9, 10],
]);
function even(numbers) {
  return numbers.filter((number) => number % 2 === 0);
}
// 计算属性中使用 reverse() 和 sort() 要注意，这两个方法将变更原始数组，计算函数中不应该这么做。请在调用这些方法之前创建一个原数组的副本。

// return [...numbers].reverse;

function warn(message, event) {
  if (event) {
    console.log('event::>>>', event);
    event.preventDefault();
  }
}

const selected = ref('A');
const options = ref([
  { text: 'One', value: 'A' },
  { text: 'Two', value: 'B' },
  { text: 'Three', value: 'C' },
  { text: 'Four', value: 'D' },
]);

const question = ref('');
const answer = ref('Questions usually contain a question mark. ;-');
watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.indexOf('?') > -1) {
    answer.value = 'Thinking...';
    try {
      const res = await fetch('https://yesno.wtf/api');
      answer.value = (await res.json()).answer;
    } catch (error) {
      answer.value = 'Error! coluld not reach the API' + error;
    }
  }
});

// watchEffect 仅会在其同步执行期间，才追踪依赖。在使用异步回调时，只有在第一个 await 正常工作前访问到的属性才会被追踪。
// watchEffect(async () => {
//   const response = await fetch(
//     `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
//   );
//   data.value = await response.json();
// });
</script>

<template>
  <h2>抽离成外部函数得到的Mouse：{{ x }} {{ y }}</h2>
  <p>
    Ask a yes/no question:
    <input v-model="question" />
  </p>
  <p>{{ answer }}</p>

  <select v-model="selected">
    <option v-for="option in options" :value="option.value">
      {{ option.text }}
    </option>
  </select>

  <div>selected：{{ selected }}</div>
  <div>你好</div>
  <div>{{ msg }}</div>
  <div>ref0：{{ ref0 }}</div>
  <button class="btn" @click="increament">
    {{ state.count1 }}
  </button>
  <h3>{{ publishedBookMessage }}</h3>
  <!-- 下边这行为啥输出是：TT -->
  <h3>fullName:{{ fullName }}</h3>
  <li v-for="(value, key, index) in myObject">
    {{ index }}.{{ key }}: {{ value }}
  </li>
  <div v-for="n in evenNumbers">{{ n }}</div>
  <ul v-for="numbers in sets">
    <li v-for="n in even(numbers)">{{ n }}</li>
  </ul>
  <button @click="warn('Form cannot be submitted yet.', $event)">提交</button>
  <div class="boat">小船儿</div>
  <br />
  <div class="bubbles">
    <div class="bubble bubble-1"></div>
    <div class="bubble bubble-2"></div>
    <div class="bubble bubble-3"></div>
    <div class="bubble bubble-4"></div>
    <div class="bubble bubble-5"></div>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
.btn {
  border: 1px solid red;
  background: pink;
}

.boat {
  width: 100px; /* 设置船的宽度 */
  height: 50px; /* 设置船的高度 */
  background-color: red; /* 设置船的背景颜色 */
  border-radius: 0 0 50% 50%; /* 利用 border-radius 属性设置船的形状为半圆 */
  position: relative; /* 设置定位为相对定位，以便让船可以在父元素中移动 */
  top: 50%; /* 将船垂直居中 */
  left: 50%; /* 将船水平居中 */
  transform-origin: bottom center; /* 设置旋转中心为船底部中心 */
  animation: swing 2s ease-in-out infinite alternate; /* 添加动画效果 */
}

@keyframes swing {
  from {
    transform: rotate(-10deg); /* 起始状态：向左偏转 10 度 */
  }
  to {
    transform: rotate(10deg); /* 结束状态：向右偏转 10 度 */
  }
}
.bubbles {
  position: relative;
  height: 100%;
}

.bubble {
  position: absolute;
  border-radius: 50%;
  background-color: gray;
  animation: bubble 2s ease-in-out infinite;
}

.bubble-1 {
  width: 20px;
  height: 20px;
  top: 40%;
  left: 10%;
  animation: float 6s ease-in-out infinite;
}

.bubble-2 {
  width: 30px;
  height: 30px;
  top: 60%;
  left: 20%;
  animation: float 8s ease-in-out infinite;
}

.bubble-3 {
  width: 25px;
  height: 25px;
  top: 30%;
  left: 50%;
  animation: float 7s ease-in-out infinite;
}

.bubble-4 {
  width: 35px;
  height: 35px;
  top: 70%;
  left: 70%;
  animation: float 9s ease-in-out infinite;
}

.bubble-5 {
  width: 15px;
  height: 15px;
  top: 20%;
  left: 90%;
  animation: float 5s ease-in-out infinite;
}

@keyframes float {
  0% {
    transform: translate(0, 0);
  }
  50% {
    transform: translate(0, -20px);
  }
  100% {
    transform: translate(0, 0);
  }
}
@keyframes bubble {
  0% {
    transform: scale(0.9);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(0.9);
  }
}
</style>
