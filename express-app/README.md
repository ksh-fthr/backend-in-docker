# 用意してある API

次の API を用意しています。確認の際は [Postman のデータ](https://github.com/ksh-fthr/backend-in-docker/blob/main/postman/communication-check.postman_collection.json) をお試しください。

| カテゴリ   | API                          | 説明                       | HTTPメソッド | ストレージについて                            |
| ---------- | ---------------------------- | -------------------------- | ------------ | --------------------------------------------- |
| 会社DB     | http://localhost/find?id=[N] | 従業員情報をID指定して取得 | GET          | `Company` DB を対象に処理する                 |
|            | http://localhost/find        | 従業員情報を全件取得       | GET          | 同上                                          |
|            | http://localhost/register    | 従業員情報を登録           | POST         | 同上                                          |
|            | http://localhost/update      | 従業員情報をID指定して更新 | PUT          | 同上                                          |
|            | http://localhost/remove      | 従業員情報をID指定して削除 | DELETE       | 同上                                          |
| 対象DB無し | http://localhost/get         | メッセージを全件取得       | GET          | DB は持たず、メモリ上のデータを対象に処理する |
|            | http://localhost/post        | メッセージを登録           | POST         | 同上                                          |
|            | http://localhost/put         | メッセージをID指定して更新 | PUT          | 同上                                          |
|            | http://localhost/delete      | メッセージをID指定して削除 | DELETE       | 同上                                          |

