# 文件/目录链接

## 1. 软连接与硬连接

Linux中链接分为软链接和硬链接。软链接类似与Windows中的快捷方式和mac OS的替身。硬链接指可以访问到文件或目录的途径，相当于文件或目录别名。

在Linux中文件名与文件内容是分开存储的，如同C#中引用类型的堆栈内存存储方式。文件名如同一个指针指向文件内容。软硬链接也都是文件指针。不同的是软链接指向的是文件名，硬链接指向的文件内容。

<img src='https://i.loli.net/2020/02/26/HKNzPtOLFCblJTn.jpg'>

Linux中删除文件首先删除文件引用，只有文件没有引用(硬链接数=0)才会被删除。如同C#中文件对象没有引用时才会被GC。软硬链接指向同一个源文件，源文件删除后，实际删除的是源文件名，所以软链接会无法使用。硬链接仍指向源文件内容，文件并不会真的删除，所以硬链接仍然正常使用。

---

> 硬链接数

硬链接数表示有多少种方式访问到对应的目录或文件。
* 文件只能通过绝对路径访问，所以文件硬链接数一般为1，有文件硬链接同样也会增加文件硬链接数
* 目录可以通过绝对路径访问，`.`方式访问。所以目录的硬链接数至少为2(无子目录)。有直接子目录时也可以通过`..`方式访问，所以每多一个直接子目录硬链接数+1。Linux目录的硬链接数等于直接子目录数量+2。mac OS的目录硬链接数与Linux计算方式不同

### 2. ln 命令

```sh
# 命令格式
$ ln [-options] source target 
```

* `ln`命令用于创建文件链接(一般指软链接)
* `-s`表示建立软链接文件，默认建立硬链接文件
* **源文件(目录)要使用绝对路径**。否则链接文件移动后会造成链接文件指向出错而无法使用

```sh
# 在当前目录建立软链接文件ln123并指定/home/colin/Desktop/demo/123.txt
$ ln -s /home/colin/Desktop/demo/123.txt ln123
```