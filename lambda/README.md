## バックエンドのエンドポイント

### 1. 目次の作成  
(https://evhv3xgbdntfdaxddbzfmidvn40nlqpg.lambda-url.ap-northeast-1.on.aws/)  
テーマを送って目次が返ってくる。

例)
リクエスト(POST)
```json
{
    "theme" : "平安時代"
}
```
レスポンス
```json
{
    "statusCode" : 200,
    "body" : {
        "1": "平安時代の歴史と背景",
        "2": "平安時代の政治と社会",
        "3": "平安時代の文化と芸術",
        "4": "平安時代の仏教と宗教",
        "5": "平安時代の貴族と武士",
        "6": "平安時代の女性と家族"
    }
}
```
### 2. スクリプト、キーポイント、音声データの作成  (https://vtuxknnzbadlbnxufp44ybpumm0lmbcs.lambda-url.ap-northeast-1.on.aws/)  
テーマ、目次、コンテンツを送って、スクリプト、キーポイント、音声データのurl(s3)が返ってくる  

例)  
リクエスト(POST)
```json
{
  "theme": "平安時代",
  "table_of_contents": {
    "1": "平安時代の歴史と背景",
    "2": "平安時代の政治と社会",
    "3": "平安時代の文化と芸術",
    "4": "平安時代の仏教と宗教",
    "5": "平安時代の貴族と武士",
    "6": "平安時代の女性と家族"
  },
  "content": "平安時代の女性と家族"
}
```

レスポンス
```json
{        
    "statusCode": 200,
    "body": {
        "script" : script(文字列),
        "key_points": {
            "1": "平安時代の女性は、家族や社会での役割によって厳格な規則に従って生活していました。",
            "2": "女性は主に家事や子育てに従事し、夫や家族の世話をすることが求められました。",
            "3": "女性の地位は家族や夫の社会的地位に依存し、上流階級の女性はより多くの自由を享受することができました。",
            "4": "女性は結婚することで夫の家族に加わり、夫の家族の家業や家政を支える役割を果たしました。",
            "5": "平安時代の女性は、文学や芸術においても活躍し、歌や詩の創作や雅楽の演奏などに参加する機会もありました。"
        },
        "url": "s3の期限付きurl(2h)",
    },
}
```

### 3. 問題の作成  
(https://zzefhrg43bbh43zuhsdsx4jcii0fgmen.lambda-url.ap-northeast-1.on.aws/)  
スクリプト、選択問題の個数、記述問題の個数を送って問題を得る。  
例)  
リクエスト(POST)
```json
{
    "script" : "スクリプト",
    "selections" : 4,
    "descriptions" : 3
}
```
\* selectionsとdescriptionsはそれぞれ選択問題と記述問題の個数  
  
  
リスポンス
```json
{
    "statusCode" : 200,
    "body" : {
        "selection": [
        {
        "question": "囲碁の終了条件は？",
        "choices": ["石の数が多い方が勝ち", "領域の大きさが大きい方が勝ち", "相手の石を全て取った方が勝ち", "自分の石を全て配置した方が勝ち"],
        "answer": "石の数が多い方が勝ち",
        "explanation": "ゲーム終了時には、碁盤上の石の数と領域の大きさを競いますが、囲碁の終了条件は石の数が多い方が勝つことです。領域の大きさも重要ですが、石の数が決定的な要素となります。"
        },
        {
        "question": "囲碁で使用される石の色は？",
        "choices": ["赤と青", "黒と白", "緑と黄", "紫とオレンジ"],
        "answer": "黒と白",
        "explanation": "囲碁で使用される石は黒と白の2色です。黒の石と白の石が交互に碁盤上に配置されます。石の色でプレイヤーを区別することができます。"
        }
        ],
        "description": [
        "囲碁で勝利する上で重要な点は何か？",
        "囲碁での地の利としてどのような要素があるか？"
        ]
    }
}
```

\* selection、descriptionはそれぞれ問題が入った配列。  
\* selectionの要素は
```json
{
    "question" : "問題文"
    "choices" : ["選択肢の配列"]
    "answer" : "答えの選択肢"
    "explanation" : "解説文"
}
```
のjson  
\* descriptionの要素は問題文の文字列  
\* selectionを生成するのに時間がかかるので少ない方がいい。  

### 4. 記述問題の採点  
(https://xh44pt54jlfovp6w23ib4kb2740tyzkm.lambda-url.ap-northeast-1.on.aws/)  
問題文、解答を送れば、講評が返ってくる。  

例)  
リクエスト(POST)
```json
{
    "question" : "囲碁で勝利する上で重要な点は何か？"
    "answer" : "解答"
}
```
レスポンス
```json
{        
    "statusCode": 200,
    "body" : "講評",
}
```

### 5. 要約の作成
(https://ldaept3xtfdusr22u4e5soqk3e0yreat.lambda-url.ap-northeast-1.on.aws/)  
スクリプトを送れば、要約が返ってくる。  
(例)  
リクエスト(POST)
```
バイナリデータ
```
レスポンス
```json
{        
    "statusCode": 200,
    "body" : "要約文",
}
```