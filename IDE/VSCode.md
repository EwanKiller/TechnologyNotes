# Configuration for unity in macOS

## VS Code Plugins

- Auto - Using for C#
- C#
- C# Extensions
- C# XML Documentation Comments
- Debugger for Unity
- Unity Code Snippets
- Unity Snippets
- Unity Snippets Modified
- Unity Tools

## settings.json

- ”commons + , “ ：打开设置，然后输入mono，点击打开settings.json，输入   : 
```
"omnisharp.monoPath": "/Library/Frameworks/Mono.framework/Versions/Current/Commands/mono",
"omnisharp.useGlobalMono": "always"
```

## bash_profile

- console输入open. bash_profile,加上:
```
export FrameworkPathOverride=/Library/Frameworks/Mono.framework/Versions/Current
```

## 如何在VSCode里用markdown记事

  - 插件库里搜索 Markdown all in one 和 Markdown Preview Enhanced 