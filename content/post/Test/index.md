---
title: <Bukkit 插件开发笔记 Chapter.1> 命令参数解析
date: 2024-11-05
slug: 1
image: kotlin.png
categories:
    - Bukkit
    - Kotlin
---


``` kotlin
fun parseOptions(args: List<String>): HashMap<String, String?> {
    val options = hashMapOf<String, String?>()
    var i = 0
    while (i < args.size) {
        val arg = args[i]
        if (arg.startsWith("--") || arg.startsWith("-")) {
            val (key, value) = if (arg.contains("=")) {
                val splitArg = arg.split("=", limit = 2)
                val key = splitArg[0].removePrefix("-").removePrefix("-")
                val value = splitArg[1]
                key to value
            } else {
                val key = arg.removePrefix("-").removePrefix("-")
                val value = if (i + 1 < args.size && !args[i + 1].startsWith("-")) args[++i] else null
                key to value
            }
            options[key] = value
        }
        i++
    }
    return options
}
```

``` kotlin
fun parseContentAfterSpace(args: List<String>): String {
    var i = 0
    var content = ""
    while (i < args.size) {
        val arg = args[i]
        if (arg.startsWith("--") || arg.startsWith("-")) break
        else content += " $arg"
        i++
    }
    return content
}
```