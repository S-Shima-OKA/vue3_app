<script setup>
import { ref } from 'vue';
import Modal from './Modal.vue';

const showModal = ref(false);

/**
 * 以下52週貯金用のスクリプト
 * @param {param}
 */
const Amount = ref('');
const AmountResultRef = ref('');
const AmoutError = ref('');
let WeeklyAmountArray = [];
let AmountSummaryDateArray = [];
let OutPutSummaryArray = {};

const AmountModule = () =>{
  AmountReset();
  AmountCalculator();
  AmountData(); 
  OutPutSummaryArray = AmountSummaryDateArray.reduce((obj, key, index) =>{
    obj[key] = WeeklyAmountArray[index];
    return obj
  }, {});
}
/**
 * 合計金額、テーブルの初期化。
 * @param {function}
 */
const AmountReset = () =>{
  OutPutSummaryArray = '';
  AmountResultRef.value = '';
  WeeklyAmountArray.splice(0);
  AmountSummaryDateArray.splice(0);
}
/**
 * 週毎の貯金額と貯金額の合計。
 * @param {function}
 */
const AmountCalculator = () =>{
  let WeeklyAmount;
  for(let i = 1; i <= 52; i++){
    WeeklyAmount = i * Amount.value;
    WeeklyAmountArray.push(WeeklyAmount);
  }
  const WeeklyAmountTotal = WeeklyAmountArray.reduce((sum, elm)=>{
    return sum + elm
  },0);
  AmountResultRef.value = WeeklyAmountTotal;
}
/**
 * 貯金する日付を算出。スタートの日付は計算を開始した日とする。
 * @param {function}
 */
const AmountData = () =>{
  // 現在の日付を取得して文字型に変換する
  const AmountStartDate = new Date()
  const AmountStartDateLocal = AmountStartDate.toLocaleDateString();
  const AmountStartDateString = new Date(AmountStartDateLocal)

  // 52週（357日）後の日付を取得して文字型に変換する。
  const AmountGoalDate = new Date();
  AmountGoalDate.setDate(AmountGoalDate.getDate() + 357)
  const AmountGoalDateLocal = AmountGoalDate.toLocaleDateString();
  const AmountGoalDateString = new Date(AmountGoalDateLocal)

  for(let WeekDate = AmountStartDateString; WeekDate <= AmountGoalDateString; WeekDate.setDate(AmountStartDateString.getDate() + 7)){
      const AmountCalcDate = `${WeekDate.getFullYear()}-${(WeekDate.getMonth()+1)}-${WeekDate.getDate()}`;
      AmountSummaryDateArray.push(AmountCalcDate)
  }
}
/**
 * エラー文言の表示と要素の初期化。
 * @param {function}
 */
const CalError = () =>{
  AmountReset();
  AmoutError.value.classList.remove('hidden');
}
/**
 * 計算イベント。フォームの値が0もしくはnull、全角文字の場合エラーを返す。
 * @param {function}
 */
const CaclStart = () =>{
  if(Amount.value <= 0 || Amount.value.match(/^[^\x01-\x7E\uFF61-\uFF9F]+$/)){
    CalError();
  } else{
    AmountModule();
    AmoutError.value.classList.add('hidden');
  }
}
/**
 * ブラーイベント。フォームからフォーカスが外れた際に値が0もしくはnullの場合エラーを返す。
 * @param {function}
 */
const onBlur = () =>{
  if(Amount.value <= 0 || Amount.value.match(/^[^\x01-\x7E\uFF61-\uFF9F]+$/)){
    // AmountReset();
    AmoutError.value.classList.remove('hidden');
  } else {
    AmoutError.value.classList.add('hidden');
  }
}
</script>
<template>
  <div class="text-center text-blue-700">
    <button @click="showModal = true"><b class="underline">52週間貯金チャレンジとは？<br>メリットについて</b></button>
  </div>
  <teleport to="body">
  <Modal  :show="showModal" @close="showModal = false">
    <h2 class="text-lg text-center border-b-2 border-blue-900 px-2 pb-2">今日から始められる貯金習慣</h2>
    <figure class="mt-3">
      <img src="../assets/images/chokin_woman.png" alt="" class="w-4/12 m-auto">
    </figure>
    <div class="mt-3">
      <p>「<b class="text-red-700">52週間貯金チャレンジ</b>」とは毎週、<br class="block md:hidden">貯金する額を少しずつ増やしていき、<br class="block md:hidden">1年間の貯金額の成長を見守る取り組み<br class="block md:hidden">です。</p>
    <p class="mt-1">例えばスタート（第1週目）の貯金額が<b class="text-red-700">100円</b>の場合だと2週目の貯金額は<br class="block md:hidden"><b class="text-red-700">200円</b>...3週目の貯金額は<b class="text-red-700">300円</b>...と1週間ごとにスタート時の貯金額を追加していきます。</p>
    <p class="mt-1">ゴールに近づくにつれ週ごとの貯金額はどんどん増えていきます。そして第52週目には<b class="text-red-700">5,200円</b>となっており、全ての週の貯金額を合計すると<b class="text-red-700">13万7,800円</b>貯まっているはずです。</p>
    <p class="mt-1">このチャレンジの魅力は、貯金が習慣付くことはもちろんゴールした時に想像以上のお金を貯めることが出来たと達成感を得られることです。</p>
    </div>
  </Modal>
</teleport>
  <section class="mt-4 w-full lg:w-6/12 m-auto">
    <div class="pt-12 text-2xl">
      <h2 class="border-b-2 border-blue-900 px-2 pb-4">早速計算してみる</h2>
    </div>
    <p class="text-center leading-loose mt-4">スタートの貯金額（半角数字）を<br class="block md:hidden">入力してください。<br>毎週の貯金額と52週分の貯金額の<br class="block md:hidden">合計を算出します。</p>
    <div class="-mr-2 text-center mt-4">
      <input 
      v-model="Amount" 
      @blur="onBlur" 
      class="p-2 rounded-md border-2 border-gray-600"><span class="pl-2">円</span>
      <p class="p-2 text-red-700 hidden" ref="AmoutError"><b>金額を半角数字で入力してください</b></p>
    </div>
    <div class="mt-4 text-center">
      <button 
        class="rounded-md bg-yellow-300 p-3"
        @click="CaclStart"
      >
        <b class="tracking-widest">貯金額を計算する</b>
      </button>
    </div>
    <p class="mt-8 text-center">合計 <span class="text-red-700 text-4xl font-bold">{{ AmountResultRef.toLocaleString() }}</span> 円貯まります。</p>
  </section>
  <section class="w-full lg:w-6/12 m-auto">
    <div class="pt-12 text-2xl">
      <h2 class="border-b-2 border-blue-900 px-2 pb-4">毎週の貯金額一覧</h2>
    </div>
    <table class="border-collapse w-full border-t border-b border-blue-900 mt-8">
      <tbody>
        <tr v-for="(Arrays,index,AmountWeek) in OutPutSummaryArray" class="border-t border-b border-blue-900">
          <th class="w-4/12 bg-blue-900 p-3 text-center text-white border-b border-white">{{AmountWeek+1}}週目</th>
          <td class="w-4/12 p-3 text-center border-b border-blue-900"><b>{{ index }}</b></td>
          <td class="w-4/12 p-3 text-right border-b border-blue-900">{{ Arrays.toLocaleString() }}円</td>
        </tr>
      </tbody>
    </table>
  </section>
</template>

