# ScalikeJDBCを試してみる

## ここまでの道のり

### 雛形生成

activatorのテンプレート（scalikejdbc, play, backbone.jsなどを使っている）があるので、それを使う

https://github.com/scalikejdbc/hello-scalikejdbc

```
$ activator new
> scalikejdbc-activator-template
> scalikejdbc-sample // 今回のアプリ名
```

```
$ cd scalikejdbc-sample/
$ ./activator
[scalikejdbc-sample] $ run
```

http://localhost:9000/

...時間かかる

エラーが出るので、画面の「apply this script now」をクリック（マイグレーションかかる）

ok

### 書き換えてみる

http://localhost:9000/#/companiesにアクセスして書き換え前の状態確認

書き換える

```scala:app/models/Company.scala
  def findAll()(implicit session: DBSession = autoSession): List[Company] = withSQL {
    select.from(Company as c)
      .where.withRoundBracket { _.eq(c.name, "Microsoft").or.eq(c.name, "Google") }
      .orderBy(c.id)
  }.map(Company(c)).list.apply()
```

http://localhost:9000/#/companiesにアクセスして変更されていることを確認



