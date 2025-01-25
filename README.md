# アニメ聖地観光支援SPARQLシステム

## 概要
RDFデータを活用した、アニメ聖地巡礼と周辺施設検索のための観光支援システム。

## 使用方法
1. SPARQLエンドポイントにアクセスする(http://dbpedia.org/sparql)
2. 以下の利用シナリオから目的に合うものを選択
3. シナリオに対応するSPARQLクエリをエディタにコピー＆ペースト
4. クエリを実行して結果を取得

## 利用シナリオ

### 利用場面1：近場でのアニメ聖地巡礼＋α

**使用例**: 東京駅から半径5km圏内のアニメ聖地を近い順で表示し、休憩として周辺500m以内のカフェを一覧で表示

#### 実装詳細
1. アニメ聖地検索クエリ（`near_animespot`）
   - 東京駅の座標を基準点として設定
   - HAVING句による5km圏内のフィルタリング
   - 距離順でのソート

  | フィールド | 説明 |
|------------|------|
| 聖地名     | 聖地として登録された場所の名称 |
| アニメ名   | 関連するアニメ作品のタイトル |
| 詳細情報   | 聖地に関する具体的な説明 |
| エピソード | 関連するアニメのエピソード番号（任意） |
| 時間       | シーンの時間情報（任意） |
| 住所       | 聖地の所在地 |
| 緯度経度   | 位置情報の座標 |
     

2. カフェ検索クエリ（`near_cafe`）
   - 選択した聖地(東京駅)の座標を基準点として設定
   - 500m圏内のカフェを検索
   - 距離順でのソート

| フィールド | 説明 |
|------------|------|
| 店名       | カフェの名称 |
| 住所       | カフェの所在地 |

#### カスタマイズ方法：

1. 現在地の変更

   使用例では,「アニメ聖地検索」「カフェ検索」共に現在地として「東京駅」を設定したが,以下を変更することで任意の場所を中心に検索可能
    - 変更箇所: BIND(35.6812362 AS ?tokyo_lat) BIND(139.7671248 AS ?tokyo_long)
  　- 渋谷駅(35.6580,139.7016)に書き換え

2. 検索範囲の変更

   クエリ内の数値を変更することで、検索範囲を広げる,狭めるなどの調整可能
    - 変更箇所: HAVING(?dist < 0.005)
    - 数値を変更することで検索範囲を調整可能（0.005 = 約500m）

3.施設カテゴリの変更

　上記の使用例では＋αの部分を,休憩を想定した任意の聖地中心のカフェ検索としたが,施設カテゴリ一覧のクエリ(category)を参考に変更可能
　 - 変更箇所: ?type rdfs:label "カフェ"
  
### 利用場面2：休日で友人たちとの好きなアニメの聖地巡礼＋α

**使用例１**: 複数のアニメ作品の聖地が集中するエリアを特定し、選択したエリア内の銭湯を検索

#### 実装詳細
1. エリア検索クエリ（`queries/anime_spot_areas.sparql`）
   - 市区町村単位での聖地集計
   - アニメ作品ごとの聖地数カウント
   - エリアごとの総聖地数算出

2. 銭湯検索クエリ（`queries/local_bathhouses.sparql`）
   - 選択したエリア内の銭湯を検索
   - カテゴリフィルタリング

#### 機能説明
- 取得情報（エリア）
  - 市区町村名
  - アニメ作品名と聖地数
  - エリアごとの総聖地数

- 取得情報（銭湯）
  - 施設名
  - 住所

[以下、技術仕様以降は同じ内容が続きます]
