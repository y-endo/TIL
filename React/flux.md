## FSAとは
https://github.com/redux-utilities/flux-standard-action
flux-standard-actionの略。  
Actionオブジェクトの構造は自由に作れるため、ルールを決めよう。  
必須条件
プレーンなJSオブジェクトであり、typeプロパティをもつこと。
type, error, payload, meta以外を含まないこと。
オプションの条件
error、payload、metaプロパティをもつこと。
payloadにデータをもたせる、形式はなんでもOK