# VueでDOMを参照するプロパティ
ルート  
```
this.$el
```

子要素
```
<div ref="hoge">fuga</div>

this.$refs.hoge
```

# dataの変更によるDOMの変更をキャッチする
$nextTickを使えば、dataの変更によるDOMの書き換えをキャッチできる  
```
<div v-for="item in itemData" class="hoge">

methods: {
  remake() {
    this.itemData = data;
    this.$nextTick(() => {
      console.log('.hoge changed');
    });
  }
}
```
