### Vuexの基本
Storeにstate,getters,mutations,actionsを定義する。  
Storeを注入したコンポーネント及び、子コンポーネントがStoreにアクセスできる。  

- state
	- Storeで管理するデータ
- getters
	- state内のデータの状態から算出される値
- mutations
	- stateのデータを変更する関数（非同期処理は不可）
- actions
	- コンポーネントから値を受け取って、mutationsにわたす（非同期が可能）。Promiseが返ってくる

### ステート
コンポーネントのcomputedから取得する。  
```
computed: {
	count() {
		return this.$store.state.count
	}
}
```
mapStateヘルパー  
```
import { mapState } from 'vuex';

export default {
	// mapStateの中では第一引数にstateが入ってくる
	computed: mapState({
		count: state => state.count,
		countAlias: 'count' // (state.count)
	})
}
```
算出プロパティの名前がステートサブツリーの名前と同じ場合は、文字列配列をmapStateに渡せる。  
```
computed: mapState([
	'count',
	'member'
])
```
他のローカル算出プロパティと組み合わせる場合  
```
computed: {
	localComputed () {},
	...mapState([])
}
```

### ゲッター
Store側に持たせるcomputedで、各コンポーネントから呼び出せる。  
ゲッターは第1引数にstateを受け取る。  
```
new Vuex.Store({
	state: {
		count: 1
	},
	getters: {
		countPlusOne: state => {
			return state.count + 1;
		}
	}
})
```
コンポーネントのcomputedから取得する。  
```
computed: {
	getCount() {
		return this.$store.getters.countPlusOne
	}
}
```
mapGettersヘルパー  
```
import { mapGetters } from 'vuex';

computed: {
	...mapGetters([
		'countPlusOne'
	])
}
```

### ミューテーション
```
new Vuex.Store({
	state: {
		count: 1
	},
	mutations: {
		increment (state) {
			state.count++;
		},
		hoge: (state, payload) {
			state.count += payload.amount;
		}
	}
});

store.commit('increment');
store.commit('hoge', {
	amount: 10
})
```

### アクション
ミューテーションに似ている。  
- アクションは状態を変更するのではなく、ミューテーションをコミットする。
- アクションは任意の非同期処理を含むことができる。

```
new Vuex.Store({
	state: {
		count: 1
	},
	mutations: {
		increment (state, payload) {
			state.count += payload.amount;
		}
	},
	actions: {
		incrementAsync ({ commit }) {
			setTimeout(() => {
				commit('increment', 10);
			}, 1000);
		}
	}
});

export default {
	methods: {
		...mapActions([
			'increment'
		])
	}
}
```
mapActionsヘルパー  
```
methods: {
	...mapActions(['increment'])
}

<button @click=increment({ hoge: 'fuga' })>
```