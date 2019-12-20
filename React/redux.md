## 読んだ
https://qiita.com/mpyw/items/a816c6380219b1d5a3bf
https://qiita.com/enshi/items/19b1924b72f8c2ffd1eb
https://qiita.com/suzukenz/items/40afe717029c2f8f4a54
https://qiita.com/morrr/items/2e284ae691af513edacc
https://qiita.com/cortyuming/items/bd82886ae2ec381e6edd
https://qiita.com/pullphone/items/fdb0f36d8b4e5c0ae893
https://www.wakuwakubank.com/posts/704-react-redux-connect/
https://qiita.com/tomi_linka/items/edb769bdfc80bc6c0f28

## Actionの命名規則
https://qiita.com/NewBieChan/items/cdde1e9c679ece4e6cfd
ReduxのAction名は、「システムが行うこと」ではなく「外の世界で実際に起こったこと」をベースに命名します。  

> The only way to change the state tree is to emit an action, an object describing what happened. (Redux公式より)
> Every action only describes what happened in the outside world (user wants to do this, server said that) (reduxコミッターのdtinthさんより)

この議論に乗っ取ると、例えばSNSなどのサービスで、あるユーザーがコメントを投稿する場合のAction名は、以下のようになります。
```
(x) CREATE_COMMENT
(o) POST_COMMENT
```

# mapStateToPropsとmapDispatchToProps
https://qiita.com/suzukenz/items/40afe717029c2f8f4a54