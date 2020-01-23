## Nuxt

### SSRでデータ通信
コンポーネントのasyncDataを使う。  
https://ja.nuxtjs.org/guide/async-data  
クライアント側、サーバ側どちらで動いているか判定するには  
```
asyncData(context) {
	if (process.server) // sever side
	if (process.client) // client side 
}
```