---
title: "PowerShellでハッシュを求める方法！"
emoji: "💻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

# 概要
仕事で文字列のハッシュを求める機会があったので備忘録として残します！

# 環境
- Windows 10
- PowerShell 5.1

# やり方
`Get-FileHash`コマンドを利用します。
デフォルトでSHA256で変換され、オプションでSHA1、SHA384、SHA512、MD5なども指定できます。
ファイルの中身をチェックするだけであればMD5でもよいと思います。
ファイルの中身のハッシュ化し、ファイル名や拡張子を変更してもハッシュ値に影響はないようです。
ファイルのみ対応しているようですが、ストリームにデータを渡すことで文字列もハッシュ化できるようです。

今回は"hogehoge"という文字列のハッシュ値を求めます。
サンプルに従って下記のようなコマンドを実行します。

```
$stringAsStream = [System.IO.MemoryStream]::new()
$writer = [System.IO.StreamWriter]::new($stringAsStream)
$writer.write("hogehoge")
$writer.Flush()
$stringAsStream.Position = 0
Get-FileHash -InputStream $stringAsStream | Select-Object Hash
```

すると
```
Hash
----
4C716D4CF211C7B7D2F3233C941771AD0507EA5BACF93B492766AA41AE9F720D
```
が求められました！

変換サイトなどもありますが、裏の実装がわからないのでセキュリティを懸念する場合にローカルでサクッとSHA256を求めるには便利かと思います！
