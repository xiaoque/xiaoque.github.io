---
layout: page
category: Errors
title: Jekyll install error
tag: [Jekyll,配置, 问题]
---
{% include JB/setup %}


###安装环境的过程中碰到问题:

安装了Ruby和Rubygem后开始安装jekyll,没想到出现了如下问题

````
Building native extensions.  This could take a while...
ERROR:  Error installing jekyll:
    ERROR: Failed to build gem native extension.

/System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/bin/ruby extconf.rb
creating Makefile

make "DESTDIR="
compiling porter.c
porter.c:359:27: warning: '&&' within '||' [-Wlogical-op-parentheses]
      if (a > 1 || a == 1 && !cvc(z, z->k - 1)) z->k--;
                ~~ ~~~~~~~^~~~~~~~~~~~~~~~~~~~
porter.c:359:27: note: place parentheses around the '&&' expression to silence this warning
      if (a > 1 || a == 1 && !cvc(z, z->k - 1)) z->k--;
                          ^
                   (                          )
1 warning generated.
compiling porter_wrap.c
linking shared-object stemmer.bundle
clang: error: unknown argument: '-multiply_definedsuppress' [-Wunused-command-line-argument-hard-error-in-future]
clang: note: this will be a hard error (cannot be downgraded to a warning) in the future
make: *** [stemmer.bundle] Error 1


Gem files will remain installed in /Library/Ruby/Gems/2.0.0/gems/fast-stemmer-1.0.2 for inspection.
Results logged to /Library/Ruby/Gems/2.0.0/gems/fast-stemmer-1.0.2/ext/gem_make.out
````


检查了一下版本：  
- **Ruby 2.0.0p247 (2013-06-27 revision 41674) [universal.x86_64-darwin13]**
- **gem 2.2.2**  
- **Python 2.7.5**
- **Mac OS X(10.9.2)**  



网上查询错误发现各种说法都，最后查了很多问答最终参考了这里 <https://github.com/Kapeli/cheatset/issues/2#issuecomment-37369283>   

只需将原本的安装语句 `sudo gem install jekyll`换成  
`sudo ARCHFLAGS=-Wno-error=unused-command-line-argument-hard-error-in-future gem install jekyll`