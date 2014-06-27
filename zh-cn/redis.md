# 关于本书

## 许可

本书《 The Little Redis Book 》基于 Attribution-NonCommercial 3.0 Unported license。你无须为本书付款。

你可以自由的复制，分发，修改和传阅本书。但请认可该书属于作者 Karl Seguin，并请勿将本书用于任何商业目的。

你可以在以下链接查看*完整的* **许可文档**:

<http://creativecommons.org/licenses/by-nc/3.0/legalcode>

## 关于作者

Karl Seguin 在多领域有着丰富经验。他参与贡献 OSS 项目, 还是技术文档撰写人而且偶尔做做演讲。他写了许多关于 Redis 的文档，以及一些工具。他用 Redis，为休闲游戏开发者写了一个免费的评级和统计服务: [mogade.com](http://mogade.com/).

Karl 还编写了 [The Little MongoDB Book](http://openmymind.net/2011/3/28/The-Little-MongoDB-Book/)，一本关于 MongoDB 的免费且流行的好书。*1*

你可以在 <http://openmymind.net> 找到他的 Blog，或者通过 [@karlseguin](http://twitter.com/karlseguin)在 Twitter 上关注他。

## 鸣谢

特别感谢 [Perry Neal](https://twitter.com/perryneal)， 赐予我你的视野，精神，和热情。你赐予了我无尽的力量。感恩。

## 最新版本

本书最新代码可以在这里获得:
<http://github.com/karlseguin/the-little-redis-book>

### 中文版本 ###
Karl 在 [the-little-redis-book](https://github.com/karlseguin/the-little-redis-book) 的 Github 链接中给出了 [JasonLai256](https://github.com/JasonLai256) 的 [the-little-redis-book](https://github.com/JasonLai256/the-little-redis-book) 链接。但貌似 JasonLai256 最后一次更新是2012年了。内容上也和原文稍微有点出入，并且由于本人水平有限，无法提交自信正确的内容。因此重开一项目。如果你被搜索引擎引导到本工程，在此向你致歉，并希望有能力者且有时间者一同完善和同步本工程。你可以通过我的 邮箱 <geminiyellow@gmail.com> 来联系我，或者通过 [@geminiyellow](https://twitter.com/geminiyellow) 在 Twitter 上关注我。
# 简介

在过去的几年中，在数据持久化和查询领域，技术和工具以惊人的速度在发展。可以这样说，终于再也不是关系型数据库独霸天下了，也就是说，数据库生态圈开始繁荣起来。

在众多的解决案和工具里面，对于我来说，Redis 是最激动人心的。为什么？首先因为它太容易学了。要掌握 Redis，用小时做单位足矣。其次，它在处理同一类问题的时候，用的方法基本类似。什么意思？Redis 并没有试图解决关于数据的一切问题。当你了解 Reids 之后，它能做什么不能做什么一目了然。当可以用它来做的时候，作为开发者，实在太爽了。

虽然你可以只用 Redis 结构件一个完整的系统，我想大多数人都会发现作为通用数据解决案的补充会更合适 - 不管是传统的关系型数据库，面向文档系统，或者是其他什么。它是那种用来实现特定功能的解决案。就是说，它更类似于一个索引引擎。你不会把你的整个应用都构筑在 Lucene 上。但当你需要一个好的搜索引擎的时候，它会提供更好的体验 - 不管对你还是你的用户。当然，这和 Redis 之于索引引擎之间的关系类似。

本书的目的在于为你掌握 Redis 打好基础。我们将把重点放在学习使用 Redis 的五种数据结构以及各种数据建模方法上。我们还会涉及一些关键的管理细节和调试技术。

# 开始

大家的学习方式不一样: 有些人喜欢动手实践，有些人喜欢看视频，还有些喜欢读文章。但是要理解 Redis ，没有什么会比动手实践更有效了。Redis 的安装非常容易，还有一个简单的 shell，我们可以在上面实现一切。让我们花几分钟将它安装到你机器上并运行起来。

## Windows 环境

Redis 本身并不正式支持 Windows ，但也有可用选项。你当然是不会把它用到生产环境上的，但用在开发的时候，我从没遇到过什么限制。

来自微软开源技术公司(Microsoft Open Technologies, Inc.)的副本在这里 <https://github.com/MSOpenTech/redis>。同样,该解决案并不是为了生产环境准备的。

另一个方案，已经有一段时间了，在<https://github.com/dmajkic/redis/downloads>。你可以下载最新版本(应该是列表中最上面一个)。解压 zip 文件，根据你的环境架构，选择使用 `64bit` 或 `32bit` 

## *nix 和 MacOSX 环境

对于 *nix 和 Mac 用户，users, 从源码编译应该是你最好的选择。该版本，最新可用版本，在这里下载 <http://redis.io/download>。 编写本书时，最新版本是 2.6.2；用以下命令安装该版本:

	wget http://redis.googlecode.com/files/redis-2.6.2.tar.gz
	tar xzf redis-2.6.2.tar.gz
	cd redis-2.6.2
	make

(当然，Redis 也可以通过各种包管理工具安装。比如说，MacOSX 用户通过 Homebrew 安装,只需要简单输入 `brew install redis`即可。)

如果你从源码编译，二进制文件被放在 `src` 文件夹下。进入 `src` 文件夹，通过 `cd src`。

## 运行和链接 Redis

如果一切正常，一套可用的 Redis 二进制文件将在你手中诞生。Redis 有一套可执行文件。我们主要使用 Redis 服务和 Redis 命令行界面 (Redis-cli，一个类 DOS 客户端)。先让我们启动服务。在 Window 上，双击 `redis-server`。在 *nix/MacOSX 上，执行 `./redis-server`。

如果你读一下启动信息，你会看到有个警告是关于 `redis.conf` 文件找不到的。Redis 会转而使用内建的默认项，这对我们接下来的学习毫无影响。

下一步，打开 Redis 控制台，双击 `redis-cli` (Windows) 或者执行 `./redis-cli` (*nix/MacOSX)。它将会链接到本地运行的默认服务端口上 (6379)。

你可以测试一下是否所有运转正常，在命令行界面输入 `info` 。你应该会看到一大堆的键值对，它提供了大量的关于服务状态的信息。

如果你的安装有问题，我建议你到[official Redis support group](https://groups.google.com/forum/#!forum/redis-db)去寻求帮助。

# Redis 驱动

很快你就会学到，Redis 的 API 描述做得非常好，就像代码中的一组方法一样。它非常简单并易于编程。也就是说，不管你是用命令行工具，或者用你喜欢的语言，所做的事情基本类似。因此，如果你想从一个编程语言开始学习它，完全没有问题。如果你想的话，去 [client page](http://redis.io/clients) 下载相应的驱动。

# 第一章 - 基础知识

Redis 有什么特别之处？它主要用来解决什么类型的问题？在使用过程中，开发者应该注意什么问题？在我们开始回答这些问题之前，首先让我们来了解一下，Redis 是什么。

Redis 通常被描述为一个基于内存的，可持久化的，键值对方式的存储。我觉得这个描述不太准确。Redis 确实把所有的数据都放到内存中(稍后详述)，并且它确实可以把数据写到硬盘上用以持久化，但是它不单单是一个简单的键值对存储。纠正这种误解是非常重要的，否则你对 Redis 的看法，以及对它所能解决问题的能力的理解就会变得狭隘起来。

实际上，在 Redis 提供的五种不同的数据结构中，只有一种是典型的键值对结构。深刻理解这五种数据结构，它们的工作原理，它们提供的方法，以及怎样用这些数据结构去建模，是学习理解 Redis 的关键。 首先，让我们来弄明白，这些数据结构的具体含义。

如果我们把数据结构这个概念放到关系型数据库世界的话，那么我们可以说，关系型数据库提供了唯一一种数据结构 - 表。表又复杂又灵活。基本没有什么问题是表不能处理的，包括建模，存储或者是管理数据。然而，这种通用性也不是没有缺点。比如说，并不是所有东西都是那么简单，不是那么快捷，看起来像它应有的样子一样。就算可以，我们也不会用一个大而全的结构，我们不是通常会用更小更专的结构吗？确实有些东西我们做不了(或者说，做得不好)，但是可以肯定的是这样做我们可以得到简单和快速，对吧？

具体问题具体分析？我们不就是这样写代码的吗？你当然不会对所有数据都套用哈希表，也不会用 scalar 变量。对我来说，这正是 Redis 的做法。如果你要处理 scalars, lists, hashes, 或者 sets, 为什么不把他们直接存为 scalars, lists, hashes 和 sets？为什么仅仅是为了确认存在值，要去调用比 `exists(key)` 更复杂的方式或者要用比 O(1) (不管数据量有多少，查询的时间都是固定不变的)更慢的方式？

# The Building Blocks

## Databases

Redis 对数据库的定义和你熟知的概念是一致的。数据库中包含一组数据。典型的数据库用例是，把所有应用的数据都集中起来，但是以应用为单位把数据分隔保存。

在 Redis 中，数据库定位非常简单，通过一个标识数字，默认开始标识是 `0`。如果你想切换到不同的数据库，你可以通过使用 `select` 命令。在命令行界面，输入 `select 1`。Redis 会响应一个 `OK` 信息然后你的提示符应该会变成类似 `redis 127.0.0.1:6379[1]>` 这样。如果你想切回默认数据库，只要在命令行界面输入 `select 0` 就可以了。

## Commands, Keys and Values

虽然 Redis 不单是一个键值对存储，但是其核心，Redis 提供的五种数据结构至少都有一个 key 和一个 value。在我们开始更深入的讨论之前，理解 key 和 value 是非常重要的。

Key 定义了如何标识数据块。我们以后将会经常和 Key 打交道，但是现在，只要知道 key 看起来应该有像 `users:leto` 这样的格式就可以了。这样一个 key 一看就知道这条数据中有一个叫 `leto` 的用户的相关信息。冒号没什么意义，不过对 Redis 来说，用符号分隔 key 是一般常用方式。

Values 表示 key 的实际数据。它们可以是任何类型。你可以存储字符串，整数，或序列化对象(以 JSON, XML 或者其他什么格式)。大多数情况下，Redis 会把 value 作为字节数组对待，并不关心内容到底是什么。注意，使用驱动不一样处理序列化方式可能也不一样(有些会让你自己处理)，因此本书我们只讨论字符串，整数和 JSON。

让我们开始动手试试。输入下列命令:

	set users:leto '{"name": "leto", "planet": "dune", "likes": ["spice"]}'

这是一个基本的 Redis 命令。首先我们实际执行的命令，在这里是 `set`。然后是它的参数。`set` 命令有两个参数: 我们设定的 key 和为 key 设置的 value。大多数情况下，不过不是所有，命令通常都带 key 参数(存在情况下，通常会是第一个)。猜猜怎么拿到刚才的值？你肯定知道(不知道嘛也没关系!):

	get users:leto

继续试试其他组合。Key 和 Value 是最基本的概念，`get` 和 `set` 命令是对它们最简单的操作。创建更多的 users，尝试不同类型的 key 和不同的 value。

## Querying

随着学习深入，有两件事变得越来越清楚。对 Redis 来说，key 是全部，而 value 无所谓。或者，换个说法，Redis 不允许你查询对象的值。上面的例子中，我们不可能查询那些生活在 `dune` 行星上的用户。

对一些人来说，这可能会造成些许困惑。我们的世界中，数据查询是那么灵活那么强大，可是 Redis 的做法看起来太原始太不务实了。不要被这种旧观念困扰你太久。记住，Redis 不是一揽子解决案。有些东西并不属于这里(由于查询的限制)。这样，在这种观念的引导下，在面临某些问题时，你会找到新的建模方案。

我们之后会看到更多的具体例子，不过重点在于我们应该理解 Redis 的这些基本事实。这有助于我们明白为什么 value 可以是任何类型 -  Redis 根本不需要去读取或者理解他们。同样，这会帮助我们用新思维在这新世界考虑新的建模方案。

## Memory and Persistence

之前我们提到过，Redis 是一个基于内存的持久化存储。对于持久化，默认情况下，Redis 基于一定量 key 的变更，来触发对数据库进行快照，保存到硬盘上。你可以配置它，比如每 Y 秒钟内，如果有 X 个 key 改变了，那么将数据保存下来。默认情况下，Redis 会在每 60 秒，如果有 1000 及以上个 key 发生改变，将对数据快照保存。或每15分钟，即使少于9个 key 发生改变，也会把数据快照保存。

另外(或者和快照一起)，Redis 支持增量模式。一旦 key 发生变化，一个增量包会更新到硬盘上。某些情况下，允许数据60秒的更新延迟，用以换取性能上的提升，是值得的，虽然有可能会发生硬件或软件异常，导致数据丢失。在某些情况下确难以接受。Redis 还有一种可选方案，我们将会在第六章看到第三种选择，将持久化任务分流到从服务器上。

至于内存，Redis 把所有的数据都保存在内存中。这说明了使用 Redis 的成本并不低: RAM 仍然还是服务器硬件中最贵的部分。

我觉得应该有些开发者对数据会占用多少空间没什么概念。莎士比亚全集大概需要 5.5MB 的存储空间。至于扩展，其他方案倾向于IO-绑定 或者CPU-绑定。这些限制(RAM 或 IO)根据数据类型和你如何去排序和查询，会要求你把数据扩展切分到更多的机器上。除非你保存巨大的媒体文件到 Redis 中，否则基于内存的存储应该没有什么问题。而对 App 来说，这是个问题，你应该会倾向于用内存-绑定来取代IO-绑定 。

Redis 还支持虚拟内存。但是，这个功能貌似是失败了(Redis 开发者自己说的)，关于它的使用已经被声明为过期了。

(另一角度看，5.5MB 大小的莎士比亚全集可以压缩到 2MB。可是 Redis 不会自动执行压缩，你需要自己处理它。因为 Redis 把 value 作为字节数组来处理，没什么理由不让你通过压缩/解压数据来换取 RAM 。)

## Putting It Together

我们谈到了许多高层面的话题。在深入 Redis 之前，我想做的最后一件事情是把这些话题整合起来。具体来说，包括查询限制，数据结构和 Redis 用内存保存数据的方式。

当你把三件事情整合起来的时候，你得到一个很棒的结果:速度。有些人会这样认为，"Redis 当然会快啊，把所有的东西都放在内存了。" 不过这仅仅是一方面。Redis 与其他解决案相比的闪光点在于它特别的数据结构。

有多快？这取决于多方面 - 你用的是哪个命令，数据的类型，等等。不过测量 Redis 的性能通常可以用**每秒**执行多少万，或者多少十万次为单位来表示。你可以自己试着执行 `redis-benchmark` (和 `redis-server` 及 `redis-cli` 在同一文件夹下) 来测试它。

我曾经尝试过把一组使用传统建模的代码转换到 Redis 上。一个负载测试，在关系模型中它花了五分钟跑完。而在 Redis 中，它只用了大概 150ms。当然你不能期望所有的转换都能得到那么大的收益，但是我希望这能给你一个概念，我们说的速度的改变是什么。

理解 Redis 的这个特性非常重要，因为它会影响你怎么和它进行交互。有 SQL 背景的开发者通常会最小化跟数据库之间的来回交互次数。这对所有的系统都是一个好习惯，包括 Redis。 但是，由于我们简单的数据结构，有时候为达成我们的查询目标，我们需要多次查询 Redis 服务。这种数据访问方式，刚开始的时候可能会觉得不太自然，但是对于我们所能获取的性能来说，其损失真的是微不足道。

## 小结

虽然我们几乎把 Redis 特性都介绍一遍，展开了很宽泛的讨论。不过别担心，弄不清楚也不要紧 - 比如说查询。在下一章我们将动手做，实践找出那些你想得到答案的所有问题。

这章中我们应该明白的几个点:

* Keys 是用来标识数据块(values)的字符串

* Values 是一串任意字节数组，Redis 不关心这个

* Redis 提供了 (实现了) 五种指定数据结构

* 综上，这让 Redis 易于使用并且很快，但是并不适用所有场景

# 第二章 - 数据结构

现在是时候开始学习 Redis 的五种数据结构了。我们将会解释每种数据结构到底是什么，提供了什么方法，以及它们适用于何种类型的功能/数据。

到目前为止，我们理解的 Redis 结构包括命令，key 和 value。关于数据结构我们并没有涉及。在我们使用 `set` 的时候，Redis 是怎么知道用了何种数据结构的？实际上所有的命令都对应到了具体的数据结构上。比如说当你用 `set` 你会把 value 储存为字符串数据结构。当你用 `hset` 你会把它储存为一个哈希。由于 Redis 的关键字集很小，所以这是完全可以掌握的。

**[Redis' website](http://redis.io/commands) 的引用文档非常好。在这里没有必要再重复一次他们已经完成的工作。我们只介绍那些在理解数据结构时必须的最重要的命令。**

这里没有比实践更有意思更重要了。你可以通过 `flushdb` 把数据库中的数据全部擦除，所以，别害羞我的小女孩，摇起来吧！

## Strings

字符串是 Redis 中最基本的数据结构。当你说键值对的时候，你肯定想到的是字符串。不要被名字迷惑，如前述，你的 value 可以是任何东西。我宁愿把它们叫做标量(Scalars)，不过大概只有我才这样。

我们已经看过一个用字符串的一般用例了，通过 key 保存对象实例。我们以后会经常用到类似这样的用法:

	set users:leto '{"name": leto, "planet": dune, "likes": ["spice"]}'

Additionally, Redis lets you do some common operations. For example `strlen <key>` can be used to get the length of a key's value; `getrange <key> <start> <end>` returns the specified range of a value; `append <key> <value>` appends the value to the existing value (or creates it if it doesn't exist already). Go ahead and try those out. This is what I get:

	> strlen users:leto
	(integer) 50

	> getrange users:leto 31 48
	"\"likes\": [\"spice\"]"

	> append users:leto " OVER 9000!!"
	(integer) 62

Now, you might be thinking, that's great, but it doesn't make sense. You can't meaningfully pull a range out of JSON or append a value. You are right, the lesson here is that some of the commands, especially with the string data structure, only make sense given specific type of data.

Earlier we learnt that Redis doesn't care about your values. Most of the time that's true. However, a few string commands are specific to some types or structure of values. As a vague example, I could see the above `append` and `getrange` commands being useful in some custom space-efficient serialization. As a more concrete example I give you the `incr`, `incrby`, `decr` and `decrby` commands. These increment or decrement the value of a string:

	> incr stats:page:about
	(integer) 1
	> incr stats:page:about
	(integer) 2

	> incrby ratings:video:12333 5
	(integer) 5
	> incrby ratings:video:12333 3
	(integer) 8

As you can imagine, Redis strings are great for analytics. Try incrementing `users:leto` (a non-integer value) and see what happens (you should get an error).

A more advanced example is the `setbit` and `getbit` commands. There's a [wonderful post](http://blog.getspool.com/2011/11/29/fast-easy-realtime-metrics-using-redis-bitmaps/) on how Spool uses these two commands to efficiently answer the question "how many unique visitors did we have today". For 128 million users a laptop generates the answer in less than 50ms and takes only 16MB of memory.

It isn't important that you understand how bitmaps work, or how Spool uses them, but rather to understand that Redis strings are more powerful than they initially seem. Still, the most common cases are the ones we gave above: storing objects (complex or not) and counters. Also, since getting a value by key is so fast, strings are often used to cache data.

## Hashes

Hashes are a good example of why calling Redis a key-value store isn't quite accurate. You see, in a lot of ways, hashes are like strings. The important difference is that they provide an extra level of indirection: a field. Therefore, the hash equivalents of `set` and `get` are:

	hset users:goku powerlevel 9000
	hget users:goku powerlevel

We can also set multiple fields at once, get multiple fields at once, get all fields and values, list all the fields or delete a specific field:

	hmset users:goku race saiyan age 737
	hmget users:goku race powerlevel
	hgetall users:goku
	hkeys users:goku
	hdel users:goku age

As you can see, hashes give us a bit more control over plain strings. Rather than storing a user as a single serialized value, we could use a hash to get a more accurate representation. The benefit would be the ability to pull and update/delete specific pieces of data, without having to get or write the entire value.

Looking at hashes from the perspective of a well-defined object, such as a user, is key to understanding how they work. And it's true that, for performance reasons, more granular control might be useful. However, in the next chapter we'll look at how hashes can be used to organize your data and make querying more practical. In my opinion, this is where hashes really shine.

## Lists

Lists let you store and manipulate an array of values for a given key. You can add values to the list, get the first or last value and manipulate values at a given index. Lists maintain their order and have efficient index-based operations. We could have a `newusers` list which tracks the newest registered users to our site:

	lpush newusers goku
	ltrim newusers 0 49

First we push a new user at the front of the list, then we trim it so that it only contains the last 50 users. This is a common pattern. `ltrim` is an O(N) operation, where N is the number of values we are removing. In this case, where we always trim after a single insert, it'll actually have a constant performance of O(1) (because N will always be equal to 1).

This is also the first time that we are seeing a value in one key referencing a value in another. If we wanted to get the details of the last 10 users, we'd do the following combination:

	ids = redis.lrange('newusers', 0, 9)
	redis.mget(*ids.map {|u| "users:#{u}"})

The above is a bit of Ruby which shows the type of multiple roundtrips we talked about before.

Of course, lists aren't only good for storing references to other keys. The values can be anything. You could use lists to store logs or track the path a user is taking through a site. If you were building a game, you might use one to track queued user actions.

## Sets

Sets are used to store unique values and provide a number of set-based operations, like unions. Sets aren't ordered but they provide efficient value-based operations. A friend's list is the classic example of using a set:

	sadd friends:leto ghanima paul chani jessica
	sadd friends:duncan paul jessica alia

Regardless of how many friends a user has, we can efficiently tell (O(1)) whether userX is a friend of userY or not:

	sismember friends:leto jessica
	sismember friends:leto vladimir

Furthermore we can see whether two or more people share the same friends:

	sinter friends:leto friends:duncan

and even store the result at a new key:

	sinterstore friends:leto_duncan friends:leto friends:duncan

Sets are great for tagging or tracking any other properties of a value for which duplicates don't make any sense (or where we want to apply set operations such as intersections and unions).

## Sorted Sets

The last and most powerful data structure are sorted sets. If hashes are like strings but with fields, then sorted sets are like sets but with a score. The score provides sorting and ranking capabilities. If we wanted a ranked list of friends, we might do:

	zadd friends:duncan 70 ghanima 95 paul 95 chani 75 jessica 1 vladimir

Want to find out how many friends `duncan` has with a score of 90 or over?

	zcount friends:duncan 90 100

How about figuring out `chani`'s rank?

	zrevrank friends:duncan chani

We use `zrevrank` instead of `zrank` since Redis' default sort is from low to high (but in this case we are ranking from high to low). The most obvious use-case for sorted sets is a leaderboard system. In reality though, anything you want sorted by some integer, and be able to efficiently manipulate based on that score, might be a good fit for a sorted set.

## 小结

本章从高层次来讲解了 Redis 的五种数据结构。使用 Redis 有一个很棒的特点就是，你能做的通常比你开始所认为的要来得多。对于 string 和 sorted sets ，肯定还有许多未被发现的用法。当你理解了正常的用例之后，你会发现 Redis 处理所有类型的问题都得心应手。还有，虽然 Redis 只提供了五种数据结构，以及相应的方法，但是不要觉得你需要把它们全用上。很少在建立一个功能的时候会这样做，只有某些很难的命令的时候才会考虑。

# 第三章 - 数据结构用例

In the previous chapter we talked about the five data structures and gave some examples of what problems they might solve. Now it's time to look at a few more advanced, yet common, topics and design patterns.

## Big O Notation

Throughout this book we've made references to the Big O notation in the form of O(n) or O(1). Big O notation is used to explain how something behaves given a certain number of elements. In Redis, it's used to tell us how fast a command is based on the number of items we are dealing with.

Redis documentation tells us the Big O notation for each of its commands. It also tells us what the factors are that influence the performance. Let's look at some examples.

The fastest anything can be is O(1) which is a constant. Whether we are dealing with 5 items or 5 million, you'll get the same performance. The `sismember` command, which tells us if a value belongs to a set, is O(1). `sismember` is a powerful command, and its performance characteristics are a big reason for that. A number of Redis commands are O(1).

Logarithmic, or O(log(N)), is the next fastest possibility because it needs to scan through smaller and smaller partitions. Using this type of divide and conquer approach, a very large number of items quickly gets broken down in a few iterations. `zadd` is a O(log(N)) command, where N is the number of elements already in the sorted set.

Next we have linear commands, or O(N). Looking for a non-indexed column in a table is an O(N) operation. So is using the `ltrim` command. However, in the case of `ltrim`, N isn't the number of elements in the list, but rather the elements being removed. Using `ltrim` to remove 1 item from a list of millions will be faster than using `ltrim` to remove 10 items from a list of thousands. (Though they'll probably both be so fast that you wouldn't be able to time it.)

`zremrangebyscore` which removes elements from a sorted set with a score between a minimum and a maximum value has a complexity of O(log(N)+M). This makes it a mix. By reading the documentation we see that N is the number of total elements in the set and M is the number of elements to be removed. In other words, the number of elements that'll get removed is probably going to be more significant, in terms of performance, than the total number of elements in the set.

The `sort` command, which we'll discuss in greater detail in the next chapter has a complexity of O(N+M*log(M)). From its performance characteristic, you can probably tell that this is one of Redis' most complex commands.

There are a number of other complexities, the two remaining common ones are O(N^2) and O(C^N). The larger N is, the worse these perform relative to a smaller N. None of Redis' commands have this type of complexity.

It's worth pointing out that the Big O notation deals with the worst case. When we say that something takes O(N), we might actually find it right away or it might be the last possible element.


## Pseudo Multi Key Queries

A common situation you'll run into is wanting to query the same value by different keys. For example, you might want to get a user by email (for when they first log in) and also by id (after they've logged in). One horrible solution is to duplicate your user object into two string values:

	set users:leto@dune.gov '{"id": 9001, "email": "leto@dune.gov", ...}'
	set users:9001 '{"id": 9001, "email": "leto@dune.gov", ...}'

This is bad because it's a nightmare to manage and it takes twice the amount of memory.

It would be nice if Redis let you link one key to another, but it doesn't (and it probably never will). A major driver in Redis' development is to keep the code and API clean and simple. The internal implementation of linking keys (there's a lot we can do with keys that we haven't talked about yet) isn't worth it when you consider that Redis already provides a solution: hashes.

Using a hash, we can remove the need for duplication:

	set users:9001 '{"id": 9001, "email": "leto@dune.gov", ...}'
	hset users:lookup:email leto@dune.gov 9001

What we are doing is using the field as a pseudo secondary index and referencing the single user object. To get a user by id, we issue a normal `get`:

	get users:9001

To get a user by email, we issue an `hget` followed by a `get` (in Ruby):

	id = redis.hget('users:lookup:email', 'leto@dune.gov')
	user = redis.get("users:#{id}")

This is something that you'll likely end up doing often. To me, this is where hashes really shine, but it isn't an obvious use-case until you see it.

## References and Indexes

We've seen a couple examples of having one value reference another. We saw it when we looked at our list example, and we saw it in the section above when using hashes to make querying a little easier. What this comes down to is essentially having to manually manage your indexes and references between values. Being honest, I think we can say that's a bit of a downer, especially when you consider having to manage/update/delete these references manually. There is no magic solution to solving this problem in Redis.

We already saw how sets are often used to implement this type of manual index:

	sadd friends:leto ghanima paul chani jessica

Each member of this set is a reference to a Redis string value containing details on the actual user. What if `chani` changes her name, or deletes her account? Maybe it would make sense to also track the inverse relationships:

	sadd friends_of:chani leto paul

Maintenance cost aside, if you are anything like me, you might cringe at the processing and memory cost of having these extra indexed values. In the next section we'll talk about ways to reduce the performance cost of having to do extra round trips (we briefly talked about it in the first chapter).

If you actually think about it though, relational databases have the same overhead. Indexes take memory, must be scanned or ideally seeked and then the corresponding records must be looked up. The overhead is neatly abstracted away (and they  do a lot of optimizations in terms of the processing to make it very efficient).

Again, having to manually deal with references in Redis is unfortunate. But any initial concerns you have about the performance or memory implications of this should be tested. I think you'll find it a non-issue.

## Round Trips and Pipelining

We already mentioned that making frequent trips to the server is a common pattern in Redis. Since it is something you'll do often, it's worth taking a closer look at what features we can leverage to get the most out of it.

First, many commands either accept one or more set of parameters or have a sister-command which takes multiple parameters. We saw `mget` earlier, which takes multiple keys and returns the values:

	ids = redis.lrange('newusers', 0, 9)
	redis.mget(*ids.map {|u| "users:#{u}"})

Or the `sadd` command which adds 1 or more members to a set:

	sadd friends:vladimir piter
	sadd friends:paul jessica leto "leto II" chani

Redis also supports pipelining. Normally when a client sends a request to Redis it waits for the reply before sending the next request. With pipelining you can send a number of requests without waiting for their responses. This reduces the networking overhead and can result in significant performance gains.

It's worth noting that Redis will use memory to queue up the commands, so it's a good idea to batch them. How large a batch you use will depend on what commands you are using, and more specifically, how large the parameters are. But, if you are issuing commands against ~50 character keys, you can probably batch them in thousands or tens of thousands.

Exactly how you execute commands within a pipeline will vary from driver to driver. In Ruby you pass a block to the `pipelined` method:

	redis.pipelined do
	  9001.times do
		redis.incr('powerlevel')
	  end
	end

As you can probably guess, pipelining can really speed up a batch import!

## Transactions

Every Redis command is atomic, including the ones that do multiple things. Additionally, Redis has support for transactions when using multiple commands.

You might not know it, but Redis is actually single-threaded, which is how every command is guaranteed to be atomic. While one command is executing, no other command will run. (We'll briefly talk about scaling in a later chapter.) This is particularly useful when you consider that some commands do multiple things. For example:

`incr` is essentially a `get` followed by a `set`

`getset` sets a new value and returns the original

`setnx` first checks if the key exists, and only sets the value if it does not

Although these commands are useful, you'll inevitably need to run multiple commands as an atomic group. You do so by first issuing the `multi` command, followed by all the commands you want to execute as part of the transaction, and finally executing `exec` to actually execute the commands or `discard` to throw away, and not execute the commands. What guarantee does Redis make about transactions?

* The commands will be executed in order

* The commands will be executed as a single atomic operation (without another client's command being executed halfway through)

* That either all or none of the commands in the transaction will be executed

You can, and should, test this in the command line interface. Also note that there's no reason why you can't combine pipelining and transactions.

	multi
	hincrby groups:1percent balance -9000000000
	hincrby groups:99percent balance 9000000000
	exec

Finally, Redis lets you specify a key (or keys) to watch and conditionally apply a transaction if the key(s) changed. This is used when you need to get values and execute code based on those values, all in a transaction. With the code above, we wouldn't be able to implement our own `incr` command since they are all executed together once `exec` is called. From code, we can't do:

	redis.multi()
	current = redis.get('powerlevel')
	redis.set('powerlevel', current + 1)
	redis.exec()

That isn't how Redis transactions work. But, if we add a `watch` to `powerlevel`, we can do:

	redis.watch('powerlevel')
	current = redis.get('powerlevel')
	redis.multi()
	redis.set('powerlevel', current + 1)
	redis.exec()

If another client changes the value of `powerlevel` after we've called `watch` on it, our transaction will fail. If no client changes the value, the set will work. We can execute this code in a loop until it works.

## Keys Anti-Pattern

In the next chapter we'll talk about commands that aren't specifically related to data structures. Some of these are administrative or debugging tools. But there's one I'd like to talk about in particular: the `keys` command. This command takes a pattern and finds all the matching keys. This command seems like it's well suited for a number of tasks, but it should never be used in production code. Why? Because it does a linear scan through all the keys looking for matches. Or, put simply, it's slow.

How do people try and use it? Say you are building a hosted bug tracking service. Each account will have an `id` and you might decide to store each bug into a string value with a key that looks like `bug:account_id:bug_id`. If you ever need to find all of an account's bugs (to display them, or maybe delete them if they delete their account), you might be tempted (as I was!) to use the `keys` command:

	keys bug:1233:*

The better solution is to use a hash. Much like we can use hashes to provide a way to expose secondary indexes, so too can we use them to organize our data:

	hset bugs:1233 1 '{"id":1, "account": 1233, "subject": "..."}'
	hset bugs:1233 2 '{"id":2, "account": 1233, "subject": "..."}'

To get all the bug ids for an account we simply call `hkeys bugs:1233`. To delete a specific bug we can do `hdel bugs:1233 2` and to delete an account we can delete the key via `del bugs:1233`.


## 小结

通过本章以及前一章，希望你已经开始有感觉知道应当怎么用 Redis 去处理实际问题了。还有很多的方式，你可以用来处理所有类型的东西，不过真正的关键是理解基础数据结构，并拥有那么一种视野，知道如何摆脱原有观念，利用它们来处理新问题。

# 第四章 - 数据结构以外

While the five data structures form the foundation of Redis, there are other commands which aren't data structure specific. We've already seen a handful of these: `info`, `select`, `flushdb`, `multi`, `exec`, `discard`, `watch` and `keys`. This chapter will look at some of the other important ones.

## Expiration

Redis allows you to mark a key for expiration. You can give it an absolute time in the form of a Unix timestamp (seconds since January 1, 1970) or a time to live in seconds. This is a key-based command, so it doesn't matter what type of data structure the key represents.

	expire pages:about 30
	expireat pages:about 1356933600

The first command will delete the key (and associated value) after 30 seconds. The second will do the same at 12:00 a.m. December 31st, 2012.

This makes Redis an ideal caching engine. You can find out how long an item has to live until via the `ttl` command and you can remove the expiration on a key via the `persist` command:

	ttl pages:about
	persist pages:about

Finally, there's a special string command, `setex` which lets you set a string and specify a time to live in a single atomic command (this is more for convenience than anything else):

	setex pages:about 30 '<h1>about us</h1>....'

## Publication and Subscriptions

Redis lists have an `blpop` and `brpop` command which returns and removes the first (or last) element from the list or blocks until one is available. These can be used to power a simple queue.

Beyond this, Redis has first-class support for publishing messages and subscribing to channels. You can try this out yourself by opening a second `redis-cli` window. In the first window subscribe to a channel (we'll call it `warnings`):

	subscribe warnings

The reply is the information of your subscription. Now, in the other window, publish a message to the `warnings` channel:

	publish warnings "it's over 9000!"

If you go back to your first window you should have received the message to the `warnings` channel.

You can subscribe to multiple channels (`subscribe channel1 channel2 ...`), subscribe to a pattern of channels (`psubscribe warnings:*`) and use the `unsubscribe` and `punsubscribe` commands to stop listening to one or more channels, or a channel pattern.

Finally, notice that the `publish` command returned the value 1. This indicates the number of clients that received the message.


## Monitor and Slow Log

The `monitor` command lets you see what Redis is up to. It's a great debugging tool that gives you insight into how your application is interacting with Redis. In one of your two redis-cli windows (if one is still subscribed, you can either use the `unsubscribe` command or close the window down and re-open a new one) enter the `monitor` command. In the other, execute any other type of command (like `get` or `set`). You should see those commands, along with their parameters, in the first window.

You should be wary of running monitor in production, it really is a debugging and development tool. Aside from that, there isn't much more to say about it. It's just a really useful tool.

Along with `monitor`, Redis has a `slowlog` which acts as a great profiling tool. It logs any command which takes longer than a specified number of **micro**seconds. In the next section we'll briefly cover how to configure Redis, for now you can configure Redis to log all commands by entering:

	config set slowlog-log-slower-than 0

Next, issue a few commands. Finally you can retrieve all of the logs, or the most recent logs via:

	slowlog get
	slowlog get 10

You can also get the number of items in the slow log by entering `slowlog len`

For each command you entered you should see four parameters:

* An auto-incrementing id

* A Unix timestamp for when the command happened

* The time, in microseconds, it took to run the command

* The command and its parameters

The slow log is maintained in memory, so running it in production, even with a low threshold, shouldn't be a problem. By default it will track the last 1024 logs.

## Sort

One of Redis' most powerful commands is `sort`. It lets you sort the values within a list, set or sorted set (sorted sets are ordered by score, not the members within the set). In its simplest form, it allows us to do:

	rpush users:leto:guesses 5 9 10 2 4 10 19 2
	sort users:leto:guesses

Which will return the values sorted from lowest to highest. Here's a more advanced example:

	sadd friends:ghanima leto paul chani jessica alia duncan
	sort friends:ghanima limit 0 3 desc alpha

The above command shows us how to page the sorted records (via `limit`), how to return the results in descending order (via `desc`) and how to sort lexicographically instead of numerically (via `alpha`).

The real power of `sort` is its ability to sort based on a referenced object. Earlier we showed how lists, sets and sorted sets are often used to reference other Redis objects. The `sort` command can dereference those relations and sort by the underlying value. For example, say we have a bug tracker which lets users watch issues. We might use a set to track the issues being watched:

	sadd watch:leto 12339 1382 338 9338

It might make perfect sense to sort these by id (which the default sort will do), but we'd also like to have these sorted by severity. To do so, we tell Redis what pattern to sort by. First, let's add some more data so we can actually see a meaningful result:

	set severity:12339 3
	set severity:1382 2
	set severity:338 5
	set severity:9338 4

To sort the bugs by severity, from highest to lowest, you'd do:

	sort watch:leto by severity:* desc

Redis will substitute the `*` in our pattern (identified via `by`) with the values in our list/set/sorted set. This will create the key name that Redis will query for the actual values to sort by.

Although you can have millions of keys within Redis, I think the above can get a little messy. Thankfully `sort` can also work on hashes and their fields. Instead of having a bunch of top-level keys you can leverage hashes:

	hset bug:12339 severity 3
	hset bug:12339 priority 1
	hset bug:12339 details '{"id": 12339, ....}'

	hset bug:1382 severity 2
	hset bug:1382 priority 2
	hset bug:1382 details '{"id": 1382, ....}'

	hset bug:338 severity 5
	hset bug:338 priority 3
	hset bug:338 details '{"id": 338, ....}'

	hset bug:9338 severity 4
	hset bug:9338 priority 2
	hset bug:9338 details '{"id": 9338, ....}'

Not only is everything better organized, and we can sort by `severity` or `priority`, but we can also tell `sort` what field to retrieve:

	sort watch:leto by bug:*->priority get bug:*->details

The same value substitution occurs, but Redis also recognizes the `->` sequence and uses it to look into the specified field of our hash. We've also included the `get` parameter, which also does the substitution and field lookup, to retrieve bug details.

Over large sets, `sort` can be slow. The good news is that the output of a `sort` can be stored:

	sort watch:leto by bug:*->priority get bug:*->details store watch_by_priority:leto

Combining the `store` capabilities of `sort` with the expiration commands we've already seen makes for a nice combo.

## Scan

In the previous chapter, we saw how the `keys` command, while useful, shouldn't be used in production. Redis 2.8 introduces the `scan` command which is production-safe. Although `scan` fulfills a similar purpose to `keys` there are a number of important difference. To be honest, most of the *differences* will seem like *idiosyncrasies*, but this is the cost of having a usable command.

First amongst these differences is that a single call to `scan` doesn't necessarily return all matching results. Nothing strange about paged results; however, `scan` returns a variable number of results which cannot be precisely controlled. You can provide a `count` hint, which defaults to 10, but it's entirely possible to get more or less than the specified `count`.

Rather than implementing paging through a `limit` and `offset`, `scan` uses a `cursor`. The first time you call `scan` you supply `0` as the cursor. Below we see an initial call to `scan` with an pattern match (optional) and a count hint (optional):

    scan 0 match bugs:* count 20

As part of its reply, `scan` returns the next cursor to use. Alternatively, scan returns `0` to signify the end of results. Note that the next cursor value doesn't correspond to the result number or anything else which clients might consider useful.

A typical flow might look like this:

    scan 0 match bugs:* count 2
    > 1) "3"
    > 2) 1) "bugs:125"
    scan 3 match bugs:* count 2
    > 1) "0"
    > 2) 1) "bugs:124"
    >    2) "bugs:123"

Our first call returned a next cursor (3) and one result. Our subsequent call, using the next cursor, returned the end cursor (0) and the final two results. The above is a *typical* flow. Since the `count` is merely a hint, it's possible for `scan` to return a next `cursor` (not 0) with no actual results. In other words, an empty result set doesn't signify that no additional results exist. Only a 0 cursor means that there are no additional results.

On the positive side, `scan` is completely stateless from Redis' point of view. So there's no need to close a cursor and there's no harm in not fully reading a result. If you want to, you can stop iterating through results, even if Redis returned a valid next cursor.

There are two other things to keep in mind. First, `scan` can return the same key multiple times. It's up to you to deal with this (likely by keeping a set of already seen values). Secondly, `scan` only guarantees that values which were present during the entire duration of iteration will be returned. If values get added or removed while you're iterating, they may or may not be returned. Again, this comes from `scan`'s statelessness; it doesn't take a snapshot of the existing values (like you'd see with many databases which provide strong consistency guarantees), but rather iterates over the same memory space which may or may not get modified.

In addition to `scan`, `hscan`, `sscan` and `zscan` commands were also added. These let you iterate through hashes, sets and sorted sets. Why are these needed? Well, just like `keys` blocks all other callers, so does the hash command `hgetall` and the set command `smembers`. If you want to iterate over a very large hash or set, you might consider making use of these commands. `zscan` might seem less useful since paging through a sorted set via `zrangebyscore` or `zrangebyrank` is already possible. However, if you want to fully iterate through a large sorted set, `zscan` isn't without value.

## 小结

本章主要讲了非特定数据结构命令。和其他一样，这些命令需要按需使用。不是创建一个应用或者功能时都要用到期限，发布/订阅 和/或 排序。不过最好应当知道它们的存在。并且，我们只讲了其中一部分命令。还有更多的，当你消化了本书内容之后，应当去看看[完整功能列表](http://redis.io/commands)。

# 第五章 - Lua 脚本

Redis 2.6 includes a built-in Lua interpreter which developers can leverage to write more advanced queries to be executed within Redis. It wouldn't be wrong of you to think of this capability much like you might view stored procedures available in most relational databases.

The most difficult aspect of mastering this feature is learning Lua. Thankfully, Lua is similar to most general purpose languages, is well documented, has an active community and is useful to know beyond Redis scripting. This chapter won't cover Lua in any detail; but the few examples we look at should hopefully serve as a simple introduction.

## Why?

Before looking at how to use Lua scripting, you might be wondering why you'd want to use it. Many developers dislike traditional stored procedures, is this any different? The short answer is no. Improperly used, Redis' Lua scripting can result in harder to test code, business logic tightly coupled with data access or even duplicated logic.

Properly used however, it's a feature that can simplify code and improve performance. Both of these benefits are largely achieved by grouping multiple commands, along with some simple logic, into a custom-build cohesive function. Code is made simpler because each invocation of a Lua script is run without interruption and thus provides a clean way to create your own atomic commands (essentially eliminating the need to use the cumbersome `watch` command). It can improve performance by removing the need to return intermediary results - the final output can be calculated within the script.

The examples in the following sections will better illustrate these points.

## Eval

The `eval` command takes a Lua script (as a string), the keys we'll be operating against, and an optional set of arbitrary arguments. Let's look at a simple example (executed from Ruby, since running multi-line Redis commands from its command-line tool isn't fun):

    script = <<-eos
      local friend_names = redis.call('smembers', KEYS[1])
      local friends = {}
      for i = 1, #friend_names do
        local friend_key = 'user:' .. friend_names[i]
        local gender = redis.call('hget', friend_key, 'gender')
        if gender == ARGV[1] then
          table.insert(friends, redis.call('hget', friend_key, 'details'))
        end
      end
      return friends
    eos
    Redis.new.eval(script, ['friends:leto'], ['m'])

The above code gets the details for all of Leto's male friends. Notice that to call Redis commands within our script we use the `redis.call("command", ARG1, ARG2, ...)` method.

If you are new to Lua, you should go over each line carefully. It might be useful to know that `{}` creates an empty `table` (which can act as either an array or a dictionary), `#TABLE` gets the number of elements in the TABLE, and `..` is used to concatenate strings.

`eval` actually take 4 parameters. The second parameter should actually be the number of keys; however the Ruby driver automatically creates this for us. Why is this needed? Consider how the above looks like when executed from the CLI:

    eval "....." "friends:leto" "m"
    vs
    eval "....." 1 "friends:leto" "m"

In the first (incorrect) case, how does Redis know which of the parameters are keys and which are simply arbitrary arguments? In the second case, there is no ambiguity.

This brings up a second question: why must keys be explicitly listed? Every command in Redis knows, at execution time, which keys are going to needed. This will allow future tools, like Redis Cluster, to  distribute requests amongst multiple Redis servers. You might have spotted that our above example actually reads from keys dynamically (without having them passed to `eval`). An `hget` is issued on all of Leto's male friends. That's because the need to list keys ahead of time is more of a suggestion than a hard rule. The above code will run fine in a single-instance setup, or even with replication, but won't in the yet-released Redis Cluster.

## Script Management

Even though scripts executed via `eval` are cached by Redis, sending the body every time you want to execute something isn't ideal. Instead, you can register the script with Redis and execute it's key. To do this you use the `script load` command, which returns the SHA1 digest of the script:

    redis = Redis.new
    script_key = redis.script(:load, "THE_SCRIPT")

Once we've loaded the script, we can use `evalsha` to execute it:

    redis.evalsha(script_key, ['friends:leto'], ['m'])

`script kill`, `script flush` and `script exists` are the other commands that you can use to manage Lua scripts. They are used to kill a running script, removing all scripts from the internal cache and seeing if a script already exists within the cache.

## Libraries

Redis' Lua implementation ships with a handful of useful libraries. While `table.lib`, `string.lib` and `math.lib` are quite useful, for me, `cjson.lib` is worth singling out. First, if you find yourself having to pass multiple arguments to a script, it might be cleaner to pass it as JSON:

    redis.evalsha ".....", [KEY1], [JSON.fast_generate({gender: 'm', ghola: true})]

Which you could then deserialize within the Lua script as:

    local arguments = cjson.decode(ARGV[1])

Of course, the JSON library can also be used to parse values stored in Redis itself. Our above example could potentially be rewritten as such:

      local friend_names = redis.call('smembers', KEYS[1])
      local friends = {}
      for i = 1, #friend_names do
        local friend_raw = redis.call('get', 'user:' .. friend_names[i])
        local friend_parsed = cjson.decode(friend_raw)
        if friend_parsed.gender == ARGV[1] then
          table.insert(friends, friend_raw)
        end
      end
      return friends

Instead of getting the gender from specific hash field, we could get it from the stored friend data itself. (This is a much slower solution, and I personally prefer the original, but it does show what's possible).

## Atomic

Since Redis is single-threaded, you don't have to worry about your Lua script being interrupted by another Redis command. One of the most obvious benefits of this is that keys with a TTL won't expire half-way through execution. If a key is present at the start of the script, it'll be present at any point thereafter - unless you delete it.

## Administration

The next chapter will talk about Redis administration and configuration in more detail. For now, simply know that the `lua-time-limit` defines how long a Lua script is allowed to execute before being terminated. The default is generous 5 seconds. Consider lowering it.

## 小结

本章介绍了 Redis 的 Lua 脚本功能。和其他任何事物一样，这个功能可能会被滥用。但是，谨慎使用，用以实现你所关注及自定义的命令时，不但可以简化你的代码，同时也可能会提高性能。Lua 脚本和 Redis 的其他功能/命令一样:你要作的仅仅是，如果需要，在一开始就使用它，然后你会发现它会用得越来越频繁熟练。

# 第六章 - Administration

Our last chapter is dedicated to some of the administrative aspects of running Redis. In no way is this a comprehensive guide on Redis administration. At best we'll answer some of the more basic questions new users to Redis are most likely to have.

## Configuration

When you first launched the Redis server, it warned you that the `redis.conf` file could not be found. This file can be used to configure various aspects of Redis. A well-documented `redis.conf` file is available for each release of Redis. The sample file contains the default configuration options, so it's useful to both understand what the settings do and what their defaults are. You can find it at <http://download.redis.io/redis-stable/redis.conf>.

Since the file is well-documented, we won't be going over the settings.

In addition to configuring Redis via the `redis.conf` file, the `config set` command can be used to set individual values. In fact, we already used it when setting the `slowlog-log-slower-than` setting to 0.

There's also the `config get` command which displays the value of a setting. This command supports pattern matching. So if we want to display everything related to logging, we can do:

	config get *log*

## Authentication

Redis can be configured to require a password. This is done via the `requirepass` setting (set through either the `redis.conf` file or the `config set` command). When `requirepass` is set to a value (which is the password to use), clients will need to issue an `auth password` command.

Once a client is authenticated, they can issue any command against any database. This includes the `flushall` command which erases every key from every database. Through the configuration, you can rename commands to achieve some security through obfuscation:

	rename-command CONFIG 5ec4db169f9d4dddacbfb0c26ea7e5ef
	rename-command FLUSHALL 1041285018a942a4922cbf76623b741e

Or you can disable a command by setting the new name to an empty string.

## Size Limitations

As you start using Redis, you might wonder "how many keys can I have?" You might also wonder how many fields can a hash have (especially when you use it to organize your data), or how many elements can lists and sets have? Per instance, the practical limits for all of these is in the hundreds of millions.


## Replication

Redis supports replication, which means that as you write to one Redis instance (the master), one or more other instances (the slaves) are kept up-to-date by the master. To configure a slave you use either the `slaveof` configuration setting or the `slaveof` command (instances running without this configuration are or can be masters).

Replication helps protect your data by copying to different servers. Replication can also be used to improve performance since reads can be sent to slaves. They might respond with slightly out of date data, but for most apps that's a worthwhile tradeoff.

Unfortunately, Redis replication doesn't yet provide automated failover. If the master dies, a slave needs to be manually promoted. Traditional high-availability tools that use heartbeat monitoring and scripts to automate the switch are currently a necessary headache if you want to achieve some sort of high availability with Redis.

## Backups

Backing up Redis is simply a matter of copying Redis' snapshot to whatever location you want (S3, FTP, ...). By default Redis saves its snapshot to a file named `dump.rdb`. At any point in time, you can simply `scp`, `ftp` or `cp` (or anything else) this file.

It isn't uncommon to disable both snapshotting and the append-only file (aof) on the master and let a slave take care of this. This helps reduce the load on the master and lets you set more aggressive saving parameters on the slave without hurting overall system responsiveness.

## Scaling and Redis Cluster

Replication is the first tool a growing site can leverage. Some commands are more expensive than others (`sort` for example) and offloading their execution to a slave can keep the overall system responsive to incoming queries.

Beyond this, truly scaling Redis comes down to distributing your keys across multiple Redis instances (which could be running on the same box, remember, Redis is single-threaded). For the time being, this is something you'll need to take care of (although a number of Redis drivers do provide consistent-hashing algorithms). Thinking about your data in terms of horizontal distribution isn't something we can cover in this book. It's also something you probably won't have to worry about for a while, but it's something you'll need to be aware of regardless of what solution you use.

The good news is that work is under way on Redis Cluster. Not only will this offer horizontal scaling, including rebalancing, but it'll also provide automated failover for high availability.

High availability and scaling is something that can be achieved today, as long as you are willing to put the time and effort into it. Moving forward, Redis Cluster should make things much easier.

## 小结

鉴于已经开始使用 Redis 的网站以及工程的数量级，毋庸置疑，Redis 已经可用于成产，并且已经用于生产中了。但是，对于某些工具，尤其是在安全性和可用性发面，仍然略显年轻。Redis 集群，我们应该很快就可以看到的，将帮助我们解决目前管理方面的一些挑战。

# 总结

总的来说，Redis 代表了数据处理方式的简化。它剥离了众多的复杂性和抽象性，并可与其他系统一同使用。一些场合 Redis 并不是最好选择。但在某些场合，Redis 简直就是为你的数据量身定做一样。

最后回到我一开始说的: Redis 简单易学。不停的有新技术出现，你很难说哪些值得花时间去学习。当你真正认识到 Redis 的简洁所带来的好处的时候，我由衷相信，它是你和你的团队所能做到的，在学习方面，最值得的投资之一。

----------

*1* : 中文版本 [the-little-mongodb-book](https://github.com/geminiyellow/the-little-mongodb-book/blob/master/zh-cn/mongodb.markdown)