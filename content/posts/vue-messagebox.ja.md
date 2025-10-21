+++
date = '2025-10-21T12:02:37+08:00'
draft = false
title = 'Vueを使用してポップアップコンポーネントをカプセル化する'
translationKey = 'vue-messagebox'
lang = 'ja'
+++

### 注：この記事は中国語から翻訳されています。相違点がある場合は、中国語版をご覧ください。

ポップアップウィンドウは、様々なシナリオで活用できる汎用的な機能です。一度開発するだけで何度も使用されるため、利用コストは重要です。

```vue 
<template>
    <div id="modal">
        <div id="box">
            <div id="text">{{ msg }}</div>
            <Button @click="emit('click')"></Button>
        </div>
    </div>
</template>
<script setup>
import Button from "~/components/Button.vue";
const emit = defineEmits(['click']);
defineProps({
    msg:{
        type:String,
        required:true
    }
})
</script>
<style scoped>
.modal{
    ...
}
.box{
    ...
}
.text{
    ...
}
</style>
```

開発は非常にシンプルに見えます。使い方を見てみましょう。

```vue 
<!-- App.vue -->
<template>
    <div>
        <Button @click="clickHandler">ポップアップを表示</Button>
        <MessageBox v-if="showMsg" :msg="msg" @click="clickHandler"></MessageBox>
    </div>
</template>
<script setup>
import { ref } from 'vue';
import Button from "~/components/Button.vue";
import MessageBox from "~/components/MessageBox.vue";
const showMsg=ref(false);
const msg=ref('プロンプトメッセージ');
const clickHandler = () => {
    showMsg.value=!showMsg.value
};
</script>
```

コンポーネントの呼び出しはかなり面倒に思えます。

では、コンポーネントをインポートせずにコンポーネントを使用することは可能でしょうか？

はい、可能です。

```diff
<!-- App.vue -->
<script setup>
- import { ref } from 'vue';
import Button from "~/components/Button.vue";
+ import showMsg from "~/commons/showMsg"
- import MessageBox from "~/components/MessageBox.vue";
- const showMsg=ref(false);
- const msg=ref('ポップアップを表示');
const clickHandler = () => {
-    showMsg.value=!showMsg.value;
+    showMsg('ポップアップを表示',(close)=>{
+    console.log('OKボタンをクリックした');
+    close();
+ })
};
</script>
```
関数を少し変更してみましょう。

変更後、開発はより複雑になります。

ただし、これは**一般的な機能**なので、開発コストをあまり考慮する必要はありません。

jsファイルで関数を定義します

```javascript
// ~/commons/showMsg.js
function showMsg() {

}
export default showMsg;
```

この関数は 2 つのパラメータを受け入れます。1 つはメッセージの内容、もう 1 つはイベントです。

```diff
// ~/commons/showMsg.js
- function showMsg() {
+ function showMsg(msg,clickHandler) {
  
}

export default showMsg;
```

この関数は、`MessageBox` をレンダリングするために使用されます。

`MessageBox.vue`ファイルをインポートします。

```diff
// ~/commons/showMsg.js
+ import Message from "~/components/MessageBox.vue"
function showMsg(msg,clickHandler) {
 
}
export default showMsg;
```

どのようにレンダリングされるのでしょうか？

`Vue` がどのようにレンダリングされるか見てみましょう。

```javascript
// main.js
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)

app.mount('#app')
```

Vue インスタンスを作成し、コンポーネントを渡して、ページ上のどこかにマウントします。

だからまたこれをやるんです。

```diff
// ~/commons/showMsg.js
+ import { createApp } from 'vue'
import Message from "~/components/MessageBox.vue"
function showMsg(msg,clickHandler) {
+    // 渲染MessageBox组件
+    const app =createApp(MessageBox);
}
export default showMsg;
```

ただし、id が app の div がメイン ページにマウントされているため、これを変更する必要があります。

```diff
// ~/commons/showMsg.js
import { createApp } from 'vue'
import Message from "~/components/MessageBox.vue"
function showMsg(msg,clickHandler) {
+    const div =document.createElement('div');
   // 渲染MessageBox组件
+    document.body.appendChild(div);
   const app =createApp(MessageBox);
+    app.mount(div);
}
export default showMsg;
```

ページを更新すると、ポップアップ ウィンドウが表示されますが、コンテンツがないことがわかります。

そこで、2つのプロパティを受け入れる必要があります。

```diff
// ~/commons/showMsg.js
import { createApp } from 'vue'
import Message from "~/components/MessageBox.vue"
function showMsg(msg,clickHandler) {
    const div =document.createElement('div');
    // 渲染MessageBox组件
    document.body.appendChild(div);
-    const app =createApp(MessageBox);
+    const app =createApp(MessageBox,{
+        msg,
+        onClick(){
+            clickHandler & clickHandler(()=>{
+                app.unmount();
+                div.remove();
+            });
+        }
+    });
    app.mount(div);
}
export default showMsg;
```

完了です。

これでメソッドを呼び出すことができます。

```vue
<!-- App.vue -->
<script setup>
import Button from "~/components/Button.vue";
import showMsg from "~/commons/showMsg"
const clickHandler = () => {
   showMsg.value=!showMsg.value;
   showMsg('欲显示的消息',(close)=>{
   console.log('点击了确定按钮');
   close();
})
};
</script>
```

現在、このアプローチは `showMsg.js` と `MessageBox.js` という複数のファイルに分割されているため、あまり適していません。

したがって、それらを 1 つのファイルに結合する必要があります。

```diff
// ~/commons/showMsg.js
import { createApp } from 'vue'
+ import Button from "~/components/Button.vue";
- import Message from "~/components/MessageBox.vue"
+
+ const MessageBox = {
+     props: {
+         msg: {
+             type: String,
+             required: true,
+         },
+     },
+     render(ctx) {
+         const { $props, $emit } = ctx;
+         return (
+             <div class="modal">
+                 <div class="box">
+                     <div class="text">{$props.msg}</div>
+                     <Button click={$emit('onClick')}></Button>
+                 </div>
+             </div>);
+     }
+ }
+
function showMsg(msg,clickHandler) {
    const div =document.createElement('div');
    // 渲染MessageBox组件
    document.body.appendChild(div);
    const app =createApp(MessageBox);
    const app =createApp(MessageBox,{
        msg,
        onClick(){
            clickHandler & clickHandler(()=>{
                app.unmount();
                div.remove();
            });
        }
    });
    app.mount(div);
}
export default showMsg;
```

次のステップはスタイルを追加することです。

ここでは `@styils/vue` が使用されています

```bash
$ npm instll @styils/vue
```

导入`@styils/vue`

```javascript
// showMsg.js
import { styled } from "@styils/vue";
```

CSSコードの記述

```javascript
// showMsg.js
const DivModal = styled('div', {
    ...
});
const DivBox = styled('div', {
    ...
});
const DivText = styled('div', {
    ...
});
```

`-` を含むスタイルは camelCase に変換されることに注意してください。

表示の変更

```diff
// showMsg.js
...
- return (
-     <div class="modal">
-         <div class="box">
-             <div class="text">{$props.msg}</div>
-             <Button click={$emit('onClick')}></Button>
-         </div>
-     </div>
- );
+ render(ctx) {
+     const { $props, $emit } = ctx;
+     return (
+         <DivModal>
+             <DivBox>
+                 <DivText>{$props.msg}</DivText>
+                 <Button click={$emit('onClick')}></Button>
+             </DivBox>
+         </DivModal>);
+ }
...
```

最終文書

```javascript
// showMsg.js
import { createApp } from 'vue'
import Button from "~/components/Button.vue";
import { styled } from "@styils/vue";
const DivModal = styled('div', {
    ...
});
const DivBox = styled('div', {
    ...
});
const DivText = styled('div', {
    ...
});

const MessageBox = {
    props: {
        msg: {
            type: String,
            required: true,
        },
    },
    render(ctx) {
        const { $props, $emit } = ctx;
        return (
            <DivModal>
                <DivBox>
                    <DivText>{$props.msg}</DivText>
                    <Button click={$emit('onClick')}></Button>
                </DivBox>
            </DivModal>);
    }
}

function showMsg(msg, clickHandler) {
    const div = document.createElement('div');
    document.body.appendChild(div);
    const app = createApp(MessageBox, {
        msg,
        onClick() {
            clickHandler & clickHandler(() => {
                app.unmount(div);
                div.remove();
            });
        }
    });
    app.mount(div);
}
export default showMsg;
```