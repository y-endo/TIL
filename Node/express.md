## Express
入門の参考記事  
https://gist.github.com/mitsuruog/fc48397a8e80f051a145  

### routing
```
const app = express();

// GET http://localhost:3000/
app.get('/', (req, res) => {});

// POST http://localhost:3000/books
app.post('/books', (req, res) => {});

// PUT http://localhost:3000/books/1
app.put('/books/:id', (req, res) => {});

// DELETE http://localhost:3000/books/1
app.delete('/books/:id', (req, res) => {});
```

express.Routerによるroutingのモジュール化
```
const app = express();
const router = express.Router();

router.get('/:id', (req, res) => {
	// 何かの処理
});

module.exports = router;

---

var router = require('./router');
...
app.use('/books', router); 
```

### セッション
express-sessionを使うと簡単にセッションを扱える。  
```
インスタンス名.use(session({
  設定項目: '値',
}))

request.session.[variable] = 'hoge';
```

### POSTで受け取ったbodyが空になっている
https://blog.ryo4004.net/web/306/  
body-parserのjson()を実行しないと空になるらしい。  