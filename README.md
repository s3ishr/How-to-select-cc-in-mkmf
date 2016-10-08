# mkmf はどのように CC を選ぶのか

## 概要

レンタルサーバーに [redcarpet](https://github.com/vmg/redcarpet) をインストールしようとしたら `CC=gcc-3.4` に固定されてしまったときの備忘録です。

## TL;DR

mkmf は `RbConfig::MAKEFILE_CONFIG` の情報を元に Makefile を生成する。  
`RbConfig::MAKEFILE_CONFIG` には Ruby を build したときの情報が入っていて、たとえば `configure_args` などが定義されている。  

[ruby:lib/mkmf.rb](https://github.com/ruby/ruby/blob/trunk/lib/mkmf.rb#L1960) によれば、`CC` は `RbConfig::MAKEFILE_CONFIG` の情報をそのまま使う。  
つまり、Ruby を build した CC の名前が `gem install` に影響するため、CC に変な名前を指定してはいけない。

redcarpet は `-fvisibility=hidden` を利用する（GCC4 以降の環境が必要）なため、以下のように細工することにした。

```
$ ls -lA ~/bin
total 0
lrwxrwxrwx  1 user group 28 Oct  8 10:16 cpp_3.4 -> /usr/local/gcc-4.y.z/bin/cpp
lrwxrwxrwx  1 user group 28 Oct  8 10:16 gcc_3.4 -> /usr/local/gcc-4.y.z/bin/gcc
```
