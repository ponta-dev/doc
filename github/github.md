# 運用
1. github上で新規ブランチの作成
2. コンソールからフェッチ
3. gitのチェックアウトで新しいブランチを選択
4. プロジェクトの作成
5. ToDoリストのカードからissueを作成
6. issueに対する作業を実施
7. すべてのissueが実施済みになったらプルリクエストを作成
8. 関連するissueをリンク、close?
9. マージリクエストの実行、リモートブランチの削除
10. git fetch とローカルブランチのチェックアウト先変更
11. 使わなくなったローカルブランチを削除する


# ブランチ
## githubブランチ
Code > Branch > 任意の名前  
で作れる  
作ったブランチに対してローカルをチェックアウトして追随  
そのあと任意のコミット、プッシュを行い、最終的にプルリクエストでマスターを更新する
## 一覧表示
`git branch`
## 作成
`git branch branch1`
## ブランチの移動
`git checkout branch1`
## ブランチへのプッシュ
`git push origin branch1`
// local origin > remote branch1

# Issues
## issueとcommitのリンク
コミットメッセージに`#issueNo`でissueとの紐づけができる
## pull requestとのリンク
`# `でリンク先が出てくるので任意に選択
# マイルストーンとのリンク
issueに対してマイルストーンを設定できる
リンクはGUI上から実行

# Pull request
## 作成
新しいブランチを作成し、コミットを重ねる  
そのあとプルリクエストを作成して`Merge pull request`でマージ

