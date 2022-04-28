---
title: "使用 Jekyll 和 GitHub Pages 制作个人博客"
date: 2022-04-27 22:00:00 +0800
tags: jekyll github-page
comment: false
show_author_profile: true
show_subscribe: false
---

### 环境

- Windows 11 家庭版
- Ruby 3.1.2
- RubyGems 3.3.7
- Jekyll 4.2.2

### 安装 Jekyll

在官方文档中有详细的安装 Jekyll 的流程说明。

Jekyll 官方文档的中文版：[http://jekyllcn.com/](http://jekyllcn.com/)。

第 1 步，安装 Ruby。

Ruby 官方网站的中文版：[http://www.ruby-lang.org/zh_cn/](http://www.ruby-lang.org/zh_cn/)。

进入 Ruby 下载页面：[http://www.ruby-lang.org/zh_cn/downloads/](http://www.ruby-lang.org/zh_cn/downloads/)

下载页面的文档指出，Windows 可以用 RubyInstaller：[https://rubyinstaller.org/](https://rubyinstaller.org/)。

> 安装 Ruby 的方法
> 
> 每个流行的平台都有多种工具可用于安装 Ruby：
> 
> - Linux/UNIX 平台，可以使用第三方工具（如 rbenv 或 RVM）或使用系统中的包管理系统。
> - macOS 平台，可以使用第三方工具（如 rbenv 或 RVM）。
> - Windows 平台，可以使用 RubyInstaller。

进入 RubyInstaller 下载页面，页面右侧的说明会告诉你该下哪一个版本的。

> Which version to download?
> 
> If you don’t know what version to install and you’re getting started with Ruby, we recommend that you use the Ruby+Devkit 3.1.X (x64) installer.

下载 Ruby+Devkit 3.1.2-1 (x64) 完成后，双击运行，启动 Ruby 安装向导，然后一路 next 即可。

记得勾选 Add Ruby executables to your PATH，把 Ruby 的运行目录添加到 Windows 的环境变量 PATH 里，要不然在命令行窗口（CMD）里不能直接用命令。

安装过程中，安装向导可能会卡住，等一会就行。RubyInstaller 会把 Ruby 和 RubyGems 一起装了。RubyGems 是一个 Ruby 程序，用来管理 Ruby 包的。

安装完成后，可以在命令行里使用命令输出一下版本信息，看看安装是否成功。

```
> ruby -v
ruby 3.1.2p20 (2022-04-12 revision 4491bb740a) [x64-mingw-ucrt]

> gem -v
3.3.7
```

第 2 步，安装 Jekyll。

使用 RubyGems 安装 Jekyll。

```
> gem install jekyll
Fetching unicode-display_width-1.8.0.gem
Fetching terminal-table-2.0.0.gem
Fetching safe_yaml-1.0.5.gem
Fetching rouge-3.28.0.gem
Fetching forwardable-extended-2.6.0.gem
Fetching pathutil-0.16.2.gem
Fetching mercenary-0.4.0.gem
Fetching liquid-4.0.3.gem
Fetching kramdown-2.4.0.gem
Fetching kramdown-parser-gfm-1.1.0.gem
Fetching ffi-1.15.5-x64-mingw-ucrt.gem
Fetching rb-inotify-0.10.1.gem
Fetching rb-fsevent-0.11.1.gem
Fetching listen-3.7.1.gem
Fetching jekyll-watch-2.2.1.gem
Fetching sassc-2.4.0.gem
Fetching jekyll-sass-converter-2.2.0.gem
Fetching concurrent-ruby-1.1.10.gem
Fetching i18n-1.10.0.gem
Fetching http_parser.rb-0.8.0.gem
Fetching eventmachine-1.2.7.gem
Fetching em-websocket-0.5.3.gem
Fetching colorator-1.1.0.gem
Fetching public_suffix-4.0.7.gem
Fetching jekyll-4.2.2.gem
Fetching addressable-2.8.0.gem
Successfully installed unicode-display_width-1.8.0
Successfully installed terminal-table-2.0.0
Successfully installed safe_yaml-1.0.5
Successfully installed rouge-3.28.0
Successfully installed forwardable-extended-2.6.0
Successfully installed pathutil-0.16.2
Successfully installed mercenary-0.4.0
Successfully installed liquid-4.0.3
Successfully installed kramdown-2.4.0
Successfully installed kramdown-parser-gfm-1.1.0
Successfully installed ffi-1.15.5-x64-mingw-ucrt
Successfully installed rb-inotify-0.10.1
Successfully installed rb-fsevent-0.11.1
Successfully installed listen-3.7.1
Successfully installed jekyll-watch-2.2.1
Temporarily enhancing PATH for MSYS/MINGW...
Building native extensions. This could take a while...
Successfully installed sassc-2.4.0
Successfully installed jekyll-sass-converter-2.2.0
Successfully installed concurrent-ruby-1.1.10
Successfully installed i18n-1.10.0
Building native extensions. This could take a while...
Successfully installed http_parser.rb-0.8.0
Building native extensions. This could take a while...
Successfully installed eventmachine-1.2.7
Successfully installed em-websocket-0.5.3
Successfully installed colorator-1.1.0
Successfully installed public_suffix-4.0.7
Successfully installed addressable-2.8.0
Successfully installed jekyll-4.2.2
Parsing documentation for unicode-display_width-1.8.0
Installing ri documentation for unicode-display_width-1.8.0
Parsing documentation for terminal-table-2.0.0
Installing ri documentation for terminal-table-2.0.0
Parsing documentation for safe_yaml-1.0.5
Installing ri documentation for safe_yaml-1.0.5
Parsing documentation for rouge-3.28.0
Installing ri documentation for rouge-3.28.0
Parsing documentation for forwardable-extended-2.6.0
Installing ri documentation for forwardable-extended-2.6.0
Parsing documentation for pathutil-0.16.2
Installing ri documentation for pathutil-0.16.2
Parsing documentation for mercenary-0.4.0
Installing ri documentation for mercenary-0.4.0
Parsing documentation for liquid-4.0.3
Installing ri documentation for liquid-4.0.3
Parsing documentation for kramdown-2.4.0
Installing ri documentation for kramdown-2.4.0
Parsing documentation for kramdown-parser-gfm-1.1.0
Installing ri documentation for kramdown-parser-gfm-1.1.0
Parsing documentation for ffi-1.15.5-x64-mingw-ucrt
Installing ri documentation for ffi-1.15.5-x64-mingw-ucrt
Parsing documentation for rb-inotify-0.10.1
Installing ri documentation for rb-inotify-0.10.1
Parsing documentation for rb-fsevent-0.11.1
Installing ri documentation for rb-fsevent-0.11.1
Parsing documentation for listen-3.7.1
Installing ri documentation for listen-3.7.1
Parsing documentation for jekyll-watch-2.2.1
Installing ri documentation for jekyll-watch-2.2.1
Parsing documentation for sassc-2.4.0
Installing ri documentation for sassc-2.4.0
Parsing documentation for jekyll-sass-converter-2.2.0
Installing ri documentation for jekyll-sass-converter-2.2.0
Parsing documentation for concurrent-ruby-1.1.10
Installing ri documentation for concurrent-ruby-1.1.10
Parsing documentation for i18n-1.10.0
Installing ri documentation for i18n-1.10.0
Parsing documentation for http_parser.rb-0.8.0
unknown encoding name "chunked\r\n\r\n25" for ext/ruby_http_parser/vendor/http-parser-java/tools/parse_tests.rb, skipping
Installing ri documentation for http_parser.rb-0.8.0
Parsing documentation for eventmachine-1.2.7
Installing ri documentation for eventmachine-1.2.7
Parsing documentation for em-websocket-0.5.3
Installing ri documentation for em-websocket-0.5.3
Parsing documentation for colorator-1.1.0
Installing ri documentation for colorator-1.1.0
Parsing documentation for public_suffix-4.0.7
Installing ri documentation for public_suffix-4.0.7
Parsing documentation for addressable-2.8.0
Installing ri documentation for addressable-2.8.0
Parsing documentation for jekyll-4.2.2
Installing ri documentation for jekyll-4.2.2
Done installing documentation for unicode-display_width, terminal-table, safe_yaml, rouge, forwardable-extended, pathutil, mercenary, liquid, kramdown, kramdown-parser-gfm, ffi, rb-inotify, rb-fsevent, listen, jekyll-watch, sassc, jekyll-sass-converter, concurrent-ruby, i18n, http_parser.rb, eventmachine, em-websocket, colorator, public_suffix, addressable, jekyll after 24 seconds
26 gems installed
```

安装过程需要几分钟，等它装完即可。如果长时间没有反应，可以在命令行按一下回车，不排除可能是 Windows 命令行卡住了没输出信息。

安装完成后，可以在命令行里使用命令输出一下版本信息，看看安装是否成功。

```
> jekyll -v

jekyll 4.2.2
```

### 生成模板

使用 jekyll 生成模板。

这里是在 D 盘的 workspace/github.io 目录里生成模板。

```
> jekyll new github.io
Running bundle install in D:/workspace/github.io...
  Bundler: Fetching gem metadata from https://rubygems.org/..........
  Bundler: Resolving dependencies...
  Bundler: Using bundler 2.3.7
  Bundler: Using public_suffix 4.0.7
  Bundler: Using colorator 1.1.0
  Bundler: Using concurrent-ruby 1.1.10
  Bundler: Using eventmachine 1.2.7
  Bundler: Using http_parser.rb 0.8.0
  Bundler: Using ffi 1.15.5 (x64-mingw-ucrt)
  Bundler: Using rb-fsevent 0.11.1
  Bundler: Using forwardable-extended 2.6.0
  Bundler: Using rexml 3.2.5
  Bundler: Using liquid 4.0.3
  Bundler: Using mercenary 0.4.0
  Bundler: Using rouge 3.28.0
  Bundler: Using safe_yaml 1.0.5
  Bundler: Using unicode-display_width 1.8.0
  Bundler: Using addressable 2.8.0
  Bundler: Using terminal-table 2.0.0
  Bundler: Using i18n 1.10.0
  Bundler: Using em-websocket 0.5.3
  Bundler: Using sassc 2.4.0
  Bundler: Using rb-inotify 0.10.1
  Bundler: Using jekyll-sass-converter 2.2.0
  Bundler: Using listen 3.7.1
  Bundler: Using pathutil 0.16.2
  Bundler: Using kramdown 2.4.0
  Bundler: Using jekyll-watch 2.2.1
  Bundler: Using kramdown-parser-gfm 1.1.0
  Bundler: Using jekyll 4.2.2
  Bundler: Fetching jekyll-feed 0.16.0
  Bundler: Fetching jekyll-seo-tag 2.8.0
  Bundler: Installing jekyll-feed 0.16.0
  Bundler: Installing jekyll-seo-tag 2.8.0
  Bundler: Fetching minima 2.5.1
  Bundler: Installing minima 2.5.1
  Bundler: Bundle complete! 7 Gemfile dependencies, 31 gems now installed.
  Bundler: Use `bundle info [gemname]` to see where a bundled gem is installed.
New jekyll site installed in D:/workspace/github.io.
```

Bundler 可以认为是一个针对项目的包管理程序。它通过项目目录下的 Gemfile 文件来管理项目依赖。Bundler 在安装依赖时，会使用 RubyGems。

### 开发服务器

Jekyll 集成了一个开发用的服务器，可以使用浏览器在本地进行预览。

```
> jekyll serve
Configuration file: D:/workspace/github.io/_config.yml
            Source: D:/workspace/github.io
       Destination: D:/workspace/github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.808 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'D:/workspace/github.io'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

正常情况应该是输出上面的内容，然后就用浏览器访问 http://localhost:4000/ 查看博客了。

#### 异常情况

```
> jekyll serve
Configuration file: D:/workspace/github.io/_config.yml
            Source: D:/workspace/github.io
       Destination: D:/workspace/github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.827 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'D:/workspace/github.io'
                    ------------------------------------------------
      Jekyll 4.2.2   Please append `--trace` to the `serve` command
                     for any additional information or backtrace.
                    ------------------------------------------------
C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve/servlet.rb:3:in `<top (required)>'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve.rb:179:in `require_relative'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve.rb:179:in `setup'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve.rb:100:in `process'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/command.rb:91:in `block in process_with_graceful_fail'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/command.rb:91:in `each'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/command.rb:91:in `process_with_graceful_fail'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve.rb:86:in `block (2 levels) in init_with_program'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `block in execute'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `each'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `execute'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary/program.rb:44:in `go'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary.rb:21:in `program'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/exe/jekyll:15:in `<top (required)>'
        from C:/Ruby31-x64/bin/jekyll:32:in `load'
        from C:/Ruby31-x64/bin/jekyll:32:in `<main>'
```

输出的信息提示使用 `--trace` 参数，看看执行过程中发生了什么。但是实际上下 1 行已经提示了，webrick 无法加载。

```
                    ------------------------------------------------
      Jekyll 4.2.2   Please append `--trace` to the `serve` command
                     for any additional information or backtrace.
                    ------------------------------------------------
C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
```

这里用一下 `--trace` 参数，输出没啥变化，依然提示 webrick 无法加载。

```
> jekyll serve --trace
Configuration file: D:/workspace/github.io/_config.yml
            Source: D:/workspace/github.io
       Destination: D:/workspace/github.io/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.747 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'D:/workspace/github.io'
C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve/servlet.rb:3:in `<top (required)>'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve.rb:179:in `require_relative'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve.rb:179:in `setup'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve.rb:100:in `process'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/command.rb:91:in `block in process_with_graceful_fail'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/command.rb:91:in `each'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/command.rb:91:in `process_with_graceful_fail'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve.rb:86:in `block (2 levels) in init_with_program'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `block in execute'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `each'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `execute'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary/program.rb:44:in `go'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/mercenary-0.4.0/lib/mercenary.rb:21:in `program'
        from C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/exe/jekyll:15:in `<top (required)>'
        from C:/Ruby31-x64/bin/jekyll:32:in `load'
        from C:/Ruby31-x64/bin/jekyll:32:in `<main>'
```

这时可以回到上面使用 jekyll 生成模板的地方，从输出的信息中可以发现 Bundler 并没有安装 webrick。所以这里手动执行命令安装 webrick。

```
> bundle add webrick
Fetching gem metadata from https://rubygems.org/..........
Resolving dependencies...
Fetching gem metadata from https://rubygems.org/..........
Resolving dependencies...
```

等待安装完成，应该就可以使用 `jekyll serve` 命令了。

### 推送项目到 GitHub Pages

GitHub Pages 官方文档：[https://pages.github.com/](https://pages.github.com/)

按照要求建一个名字叫 **{username}.github.io** 仓库，然后把项目推上去就行了。

然后就可以通过 **https://{username}.github.io** 访问了。