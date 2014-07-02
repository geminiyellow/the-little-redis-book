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

字符串是 Redis 中最基本的数据结构。当你说键值对的时候，你肯定想到的是字符串。不要被名字迷惑，如前述，你的 value 可以是任何东西。我宁愿把它叫标量(Scalars)，不过大概只有我才这样。

我们已经看过一个用字符串的一般用例了，通过 key 保存对象实例。我们以后会经常用到类似这样的用法:

	set users:leto '{"name": leto, "planet": dune, "likes": ["spice"]}'

另外， Redis 还有一些字符串通用操作。比如 `strlen <key>` 可以用来获取 key 的对应 value 的长度; `getrange <key> <start> <end>` 返回 key 的 value 的指定范围的值; `append <key> <value>` 追加值到当前值上 (或者不存在的时候生成)。动手试试看，下面是我得到的结果:

	> strlen users:leto
	(integer) 50

	> getrange users:leto 31 48
	"\"likes\": [\"spice\"]"

	> append users:leto " OVER 9000!!"
	(integer) 62

现在，你肯定会觉得，说得好，但这毫无意义。光从 JSON 中抽出一段范围或者追加一个值完全没有意义。你说得对，这里的想说明的是，一些命令，特别是对于字符串类型数据结构，只有在指定类型的数据中才有意义。

原先我们讲过，Redis 不关心你的值是什么。多数情况下这是对的。但是，一小部分字符串命令对于某些类型或结构的值非常有用。比如说，我们可以上面的 `append` 和 `getrange` 命令，在处理一些 custom space-efficient serialization 的时候非常有用。一个更具体的例子，你可是试试看 `incr`, `incrby`, `decr` 和 `decrby` 命令。下面字符串的值进行增减操作:

	> incr stats:page:about
	(integer) 1
	> incr stats:page:about
	(integer) 2

	> incrby ratings:video:12333 5
	(integer) 5
	> incrby ratings:video:12333 3
	(integer) 8

如你所想，Redis 的字符串结构对于分析操作非常有效。试试看 `users:leto` (非整形值) 会怎样 (你会拿到个异常)。

再来一个更高级的例子，`setbit` 和 `getbit` 命令。这有篇 [wonderful post](http://blog.getspool.com/2011/11/29/fast-easy-realtime-metrics-using-redis-bitmaps/)，关于 [Spool](http://blog.getspool.com) 如何结合使用这两个命令，来高效的回答 "今天有多少独立用户访问了我" 这个问题的。一亿两千八百万用户，在笔记本上测试，50ms 内做出了回答，而且只占用了16MB的内存。

你不明白 bitmap 的工作原理没关系，不知道 Spool 怎么用这两个命令也没关系，只想让你明白 Redis 字符串操作比看起来要强大得多得多。好了话说回来，最常用的场景是我们上面给出的场景:排序(不管简单复杂)和计数。还有，因为根据 key 拿 value 超快，所以字符串结构也通常用于缓存数据。

## Hashes

哈希结构是一个很好的例子，说明了为什么说 Redis 是个单纯的键值对存储是不对的。你看，在多数情况下，哈希结构看起来就跟字符串结构一样。但最大的不同是，它们还有另外一层中间层: 字段。所以，哈希的 `set` 和 `get` 是这样的:

	hset users:goku powerlevel 9000
	hget users:goku powerlevel

我们可以一次设定多个字段，一次获取多个字段，获取所有的字段和值，列出所有的字段清单或者删除指定字段:

	hmset users:goku race saiyan age 737
	hmget users:goku race powerlevel
	hgetall users:goku
	hkeys users:goku
	hdel users:goku age

如你所见，相比纯字符串结构，哈希结构给了我们更多的控制权限。相比把用户单纯保存为一个序列化之后的字符串，我们可以用一个哈希做更精确的描述。好处就是你可以拉取和更新/删除指定的数据片段，而不用获取或者重写整个值。

从优化定义对象的角度出发学习哈希，比如说定义一个用户，是学习理解它的工作原理的关键。而且确实，从性能方面来看，更细颗粒的操作是必须的。那么，在下一章，我们将看看怎样用哈希结构组织数据结构以及怎样用它来优化查询。在我看来，这是哈希结构真正厉害的地方。

## Lists

列表结构可以让你，为指定的 key 保存和处理数组形式的 value 。你可以向数组插入值，获取第一个或者最后一个值，以及操作指定索引位置上的值。列表结构会维护这些值的排序，并且有基于索引的高效操作。比如我们可以创建一个 `newusers` 列表用来跟踪我们网站最新的注册用户:

	lpush newusers goku
	ltrim newusers 0 49

首先我们 push 一个新用户到列表的最前面，然后我们再 trim 它，这样就只保持了最新的 50 个用户了。这是一个常见的模式。 `ltrim` 是一个 O(N) 操作，其中 N 是我们删除数据的数量。这个例子中，我们总是在一个单项插入之后做 trim ,所以它实际上会有一个恒定的 O(1) 性能(因为 N 总是等于 1)。

而下面这个例子，我们将第一次接触到，把 key 作为查询结果得到之后，再用于查询 value 的例子。比如我们想拿到最后 10 位用户的详细信息，我们可以这样操作:

	ids = redis.lrange('newusers', 0, 9)
	redis.mget(*ids.map {|u| "users:#{u}"})

上面这个 Ruby 的小例子演示了我们之前说过的多次查询操作。

当然，列表结构的好处不单单是用来保存另外的 key 引用。value 可以是任何东西。你可以用列表结构来存储日志或者跟踪用户访问网站的路径足迹。如果你用来做游戏，你可以拿来记录玩家的动作队列。

## Sets

集合结构被用于存储唯一值，并且提供了一组基于集合的操作，比如说并集运算。集合是无序的，但是它提供了许多高效的基于值的操作。朋友圈就是最经典的使用集合的例子了:

	sadd friends:leto ghanima paul chani jessica
	sadd friends:duncan paul jessica alia

不管一个用户有多少个朋友，我们都可以迅速的说出 (O(1)) userX 是否是 userY 的朋友:

	sismember friends:leto jessica
	sismember friends:leto vladimir

而且我们可以看看是否两个或者多个用户之间是否有共同好友:

	sinter friends:leto friends:duncan

甚至直接可以把这个结果存到一个新 key 中:

	sinterstore friends:leto_duncan friends:leto friends:duncan

集合非常适用于这种难解的情况:需要标记或者跟踪那些有重复属性的值的时候(或者我们希望使用集合的交并操作的时候)。

## Sorted Sets

最后一个也是最强力的一个数据结构是有序集合结构。如果说哈希结构看起来像字符串结构，但是有字段，那么有序集合结构就像集合结构一样，但是有权重(score)。权重提供了排序和排名功能。如果我们想看朋友排名，我们可以这样:

	zadd friends:duncan 70 ghanima 95 paul 95 chani 75 jessica 1 vladimir

想找出 `duncan` 有多少朋友的权重是在 90 及以上的？

	zcount friends:duncan 90 100

那怎么找出 `chani` 的排名呢？

	zrevrank friends:duncan chani

我们用 `zrevrank` 来代替 `zrank` 是因为 Redis 默认排序是从低到高的(但这里我们需要从高到低排序)。有序集合最常见的用例就是排行榜系统了。事实上，任何你想以整数做为权重排序的东西，以及那些用权重可以很好处理的操作，都适用于有序集。

## 小结

本章从概要层面来讲解了 Redis 的五种数据结构。使用 Redis 有一个很棒的特点就是，你能做的通常比你开始所认为的要来得多。对于 string 和 sorted sets ，肯定还有许多未被发现的用法。当你理解了正常的用例之后，你会发现 Redis 处理所有类型的问题都得心应手。还有，虽然 Redis 只提供了五种数据结构，以及相应的方法，但是不要觉得你需要把它们全用上。很少在建立一个功能的时候会这样做，只有某些很难的命令的时候才会考虑。

# 第三章 - 数据结构用例

上一章中我们介绍了五种数据结构并针对它们适用的情况给出了一些例子。现在我们来看看更高级，更通用，的话题和设计模式。

## Big O Notation

在本书中，我们用 `O(n)` 或 `O(1)` 来表示 `Big O notation`。Big O notation 用于表示，处理某事物时基于指定处理元素的数量，将会出现怎样特定的行为。在 Redis 中，用它来表示，基于我们处理的数据的数量，命令执行的速度将会如何。

Redis 文档给出了它的每个命令的 Big O notation。还告诉我们影响性能的因素是什么。让我们来看看例子。

最快的应该是 O(1) ，一个常量。不管我们处理五条项目还是五百万条项目，都会有同样的性能。`sismember` 命令，用于查询一个值是否属于一个集合，就是 O(1)。`sismember` 是个强力的命令，很大一个原因就是快。Redis 中的大多数命令都是 O(1)。

Logarithmic, 或者说 O(log(N)), 应该是第二快的，因为它需要扫描的区间范围越来越小。通过使用这种类型的切分和处理方法，一个非常大的集合仅需要做几次迭代就会被迅速的分解。`zadd` 是一个 O(log(N)) 命令，N 是在有序集合中的元素个数。

之后是线性复杂度，或者说 O(N)。在表中查找没有做索引的列就是一个 O(N) 操作。就像用 `ltrim` 命令一样。但是，在 `ltrim` 中，N 不是列表的元素个数，而是要移除的元素的个数。比如用 `ltrim` 从有百万项目的列表中移除一条，会比从一个只有一千条项目的列表中移除十条要快。(虽然都挺快，可能快到你根本就测不到它们的差别)。

`zremrangebyscore` 用来从有序列表中删除那些权重在最小值和最高值之间的元素，拥有复杂度 O(log(N)+M)。这有点复杂。通过查阅文档我们可以看到 N 是集合中所有的元素的个数，而 M 是需要删除的元素的个数。也就是说，在性能方面，比起集合中所有元素的个数，需要删除的元素的个数对性能影响更明显。

`sort` 命令，我们在下一章会进行更详细的讨论，在这里我们要知道它有复杂度 O(N+M*log(M))。从它的性能特点来说，我们可以这样说，它是 Redis 最复杂命令中的一个。

还有另外一些复杂度，这里还有两个比较常用的是 O(N^2) 和 O(C^N)。N 越大，性能越差。Redis 没有这种复杂度的命令。

值得指出的是，Big O notation 说的是最坏情况。比如我们说某操作的复杂性是 O(N)，那我们就有可能一开始就找到它或者在最后才找到它。


## Pseudo Multi Key Queries

一个常见的情况是，你会想用不同关键字查到同样的值。比如说，你想查找一个用户信息，通过 email (比如说他第一次登陆的时候) 或者通过 id (当他已经登陆之后)。一个很糟糕的做法是，你用两条一样的字符串来保存冗余的用户对象:

	set users:leto@dune.gov '{"id": 9001, "email": "leto@dune.gov", ...}'
	set users:9001 '{"id": 9001, "email": "leto@dune.gov", ...}'

糟糕的原因是当你要维护这些数据的时候，这绝对是个噩梦，并且它们会占用你两倍内存。

如果 Redis 允许你把一个 key 链接到映射一个的话，那就最好不过了，可是不能(并且应该永远也不可能)。Reids 开发的一个主要驱动就是要保持代码和 API 的简洁。内部实现链接 key (还有好多我们可以用 key 来做的事情没说到呢) 毫无意义，如果你意识到 Redis 提供的另一个方案的话: 哈希结构

使用哈希结构，我们可以删除冗余内容:

	set users:9001 '{"id": 9001, "email": "leto@dune.gov", ...}'
	hset users:lookup:email leto@dune.gov 9001

我们要做的仅仅是用字段作为伪二阶索引，并把它指向用户对象。如想通过 id 获取 用户，我们可以用普通的 `get`:

	get users:9001

想要通过 email 来获取用户，我们用 `hget` 配合 `get` (Ruby):

	id = redis.hget('users:lookup:email', 'leto@dune.gov')
	user = redis.get("users:#{id}")

这样的操作以后会经常用到，这就是哈希结构真正厉害的地方，不过如果你没这种需求，似乎这也不是一个很好的例子。

## References and Indexes

我们已经看过许多关于怎样用一个 value 引用另一个的例子。在的列表结构的例子中看到过，在上一节的用哈希结构优化查询的例子中也看到过。总结一下就是，必须手工维护你的 value 之间的索引和引用。老实说，我觉得这真的好不爽，特别是当你想到要手工去维护/更新/删除这些引用的时候。不过在 Redis 中确实没什么好办法。

我们已经知道集合通常是怎样实现这种手工索引了:

	sadd friends:leto ghanima paul chani jessica

该集合的每个成员都指向一条保存有实际用户信息的 Redis 字符串。但如果 `chani` 改名了怎么办，或者删掉她的账号了怎么办？也许应该再跟踪一下反向关系:

	sadd friends_of:chani leto paul

维护另说，如果你像我这样做，肯定会被这些额外的索引值的处理和内存开销给吓到。在下一章，我们将会谈谈通过使用额外的查询次数降低性能开销(我们在第一章中已经简单的提到过了)。

如果你仔细想一下，其实关系型数据库也有一样的开销。索引会占用内存，必须扫描或者定位，然后找到需要的记录。当然这些开销被抽象得很好(他们为此作了许多优化，而且运作的非常好)。

再次，在 Redis 中手工管理引用确实不幸。但是对于你所担心的性能和内存的问题，都应该先测试一下。我想你会发现其实它不是问题。

## Round Trips and Pipelining

我们已经提到过，在 Redis 中，频繁访问服务器端是很常见的模式。因为有些操作你会不停的用到，值得我们去仔细看一下我们能从哪些特性中获取更多收益。

首先，许多命令都可以接收一个或者多个参数，或者有一个带有多个参数的子查询。我们早些时候看到的 `mget` ，带有多个 key 和可以返回多个 value:

	ids = redis.lrange('newusers', 0, 9)
	redis.mget(*ids.map {|u| "users:#{u}"})

或者 `sadd` 命令向集合中添加一个或多个记录:

	sadd friends:vladimir piter
	sadd friends:paul jessica leto "leto II" chani

Redis 也支持管道。通常，一个客户端向 Redis 发送一个请求，然后在下次请求之前会一直等待返回。而用管道你可以发送一堆请求却不用等待它们的响应。这不单降低了网络开销，也在性能上有显著提高。

值得指出的是， Redis 会用内存给命令排队，因此一个好办法是给它们做批处理。你需要根据你使用的命令来决定批处理应该有多大，更具体就是，用多大的参数。不过，如果你用的是 ~50 字符长度的 key 的话，你大约可以把批处理规模放宽到几千或者上万。

在管道中执行命令的顺序根据驱动不同而不同。比如在 Ruby 中你给 `pipelined` 方法传入一个代码块:

	redis.pipelined do
	  9001.times do
		redis.incr('powerlevel')
	  end
	end

如你所想，批处理会被管道加速。

## Transactions

Redis 所有的命令都是原子性的，包括那些一次可以执行多项操作的命令也一样。此外，在使用多命令的时候，Redis 支持事务。

你可能不知道，但是 Redis 确实是单线程的，这就是为什么每个命令都是原子性的原因。一次只能执行一个命令，其他的命令不能执行。(We'll briefly talk about scaling in a later chapter.) 这在你考虑用那些一次可执行多项操作的命令时候特别有用。比如说:

`incr` 实际上是一个 `get` 后面跟个 `set`

`getset` 设置一个新值之后返回原值

`setnx` 首先检查 key 是否存在，当它不存在的时候设值

虽然这些命令很有用，但不可避免的，你肯定会遇到需要以组为单位执行多个命令的情况。首先你需要 `multi` 命令，然后接下来是你希望作为一组事务执行的所有命令，最后用 `exec` 来实际执行命令，或者用 `discard` 来放弃取消执行所有的命令。Redis 的事务可以保证什么？

* 命令将被顺序执行

* 命令组将以单原子模式执行(命令组执行途中不会插入别的客户端的命令操作)

* 在事务中的命令，要么全部执行成功，要么全部执行失败

你可以，也应该，在命令行界面测试一下这个。Also note that there's no reason why you can't combine pipelining and transactions.

	multi
	hincrby groups:1percent balance -9000000000
	hincrby groups:99percent balance 9000000000
	exec

最后，Redis 允许你指定监视一个 key(或一组 key)，如果 key(s) 改变了，那将根据情况选择执行事务。这可以用于当你在同一个事务中需要取值，并基于取得结果执行操作的情况。上面的代码中，我们不能自己实现 `incr` 命令，因为命令总是在 `exec` 执行之后一起顺序执行。用代码来说就是，我们不能这样:

	redis.multi()
	current = redis.get('powerlevel')
	redis.set('powerlevel', current + 1)
	redis.exec()

这不在 Redis 事务的责任范围之内。但是，如果我们加上 `watch` 给 `powerlevel`，我们可以这样:

	redis.watch('powerlevel')
	current = redis.get('powerlevel')
	redis.multi()
	redis.set('powerlevel', current + 1)
	redis.exec()

如果另一个客户端在调用 `watch` 之后，改变了 `powerlevel` 的话，我们的事务将会失败。如果值没有变化，那么 set 将会起作用。我们可以在循环中不断执行这段代码直到它成功为止。

## Keys Anti-Pattern

在下一章中，我们将讨论一些和具体数据结构无关的命令。某些是关于管理或者调试工具的。不过在这里有一个我特别想说一说的是: `keys` 命令。该命令通过指定模式返回所有匹配的 key。这个命令看起来在某些情况下很适用，但是它绝对不应当用在产品代码中。为什么？因为它为了查找匹配的 key 会对所有的 key 做一个线性扫描。或者，简单的说，它慢死了。

那为什么有人会尝试用它？比如说你在做一个 bug 跟踪服务。每个账户有字段 `id` ，并且你想把每个 bug 存到一个字符串值里面去，对应的 key 看起来像 `bug:account_id:bug_id`。如果你需要找出一个账号下所有的 bug (显示它们，或者删除账号之后把 bug 一同删除),你应该试试 (因为我就这样!) 使用 `keys` 命令:

	keys bug:1233:*

好一点的解决案是用哈希结构。就像我们可以用哈希来暴露二级索引那样，所以我们也可以用它来组织我们的数据:

	hset bugs:1233 1 '{"id":1, "account": 1233, "subject": "..."}'
	hset bugs:1233 2 '{"id":2, "account": 1233, "subject": "..."}'

为了取得一个账户下的所有 bug 标识符，我们只需要调用 `hkeys bugs:1233`。要删除指定 bug 我们可以 `hdel bugs:1233 2`，要删除账户的话我们可以通过 `del bugs:1233` 来删除 key。


## 小结

通过本章以及前一章，希望你已经开始有感觉知道应当怎么用 Redis 去处理实际问题了。还有很多的方式，你可以用来处理所有类型的东西，不过真正的关键是理解基础数据结构，并拥有那么一种视野，知道如何摆脱原有观念，利用它们来处理新问题。

# 第四章 - 数据结构以外

在 Redis 五种基本数据结构以外，还有一些命令是和数据结构没有关系的。我们已经看过一些了: `info`, `select`, `flushdb`, `multi`, `exec`, `discard`, `watch` 和 `keys`。本章再来看看其他的重要的一些。

## Expiration

Redis 允许你指定 key 的存活时间。你可以以 Unix 时间戳格式指定一个具体的时间 (从1970年01月01日开始的秒数)或指定以秒为单位的存活时间。这是一个基于 key 的命令，和 key 所对应的数据结构无关。

	expire pages:about 30
	expireat pages:about 1356933600

第一个命令会在三十秒后删除 key (当然包括关联的值) 。第二个会在2012年12月31日上午 12:00 删除 key。

这让 Redis 成为一个理想的缓存引擎。通过 `ttl` 命令，你可以找出一条数据还能活多久。通过 `persist` 命令你可以删除那些过期的数据:

	ttl pages:about
	persist pages:about

最后，还有一个特殊的字符串命令, `setex` 允许你在一个单独的原子命令中设置一个字符串并指定它的存活时间 (这比什么都方便):

	setex pages:about 30 '<h1>about us</h1>....'

## Publication and Subscriptions

Redis 的列表结构有 `blpop` 和 `brpop` 命令，可以从列表中返回并删除第一个(或最后一个)元素，或者堵塞到有可用元素为止。这可以用于作成简单的队列。

除此之外，Redis 对发布信息/订阅频道有着一流的支持。你可以打开第二个 `redis-cli` 窗口自己试试。首先在第一个窗口中订阅频道。(我们假设它叫 `warnings`):

	subscribe warnings

命令返回你订阅的信息。然后，在另外一个窗口中，发布一条信息到 `warnings` 频道:

	publish warnings "it's over 9000!"

如果你切回你的第一个窗口，你会发现接收到了 `warnings` 频道的消息。

你可以订阅多个频道 (`subscribe channel1 channel2 ...`)，订阅某种模式的一组频道 (`psubscribe warnings:*`) 或用 `unsubscribe` 和 `punsubscribe` 命令来停止监听一个，多个，或者某种模式的一组频道。

最后，注意 `publish` 命令的返回值 1。这是收到消息的客户端的个数。


## Monitor and Slow Log

`monitor` 命令让你监控 Redis 的状态。它是一个很棒的调试工具，能让你深入了解你的应用是怎样和 Redis 交互的。在你的两个 redis-cli 窗口中的一个 (如果它还在订阅状态，你可以用 `unsubscribe` 命令或者直接关掉窗口然后再开一个新的) 输入 `monitor` 命令。在另一个，执行其他的任意类型的命令 (比如 `get` 或者 `set`)。你可以看到这些命令，以及它们的参数，会在第一个窗口中显示。

你应该注意不要在生产环境中使用监控命令，它就是一个调试和开发的工具而已。除此之外，没得说。它就是一个很棒的开发工具。

和 `monitor`一起的，Redis 还有一个 `slowlog` ，也是一个很棒的性能分析工具。它会记录所有执行时间超过指定 **微**秒 的命令。在下一章我们会概述怎样配置 Redis，不过现在你可以像这样配置 Redis ，对所有的命令做日志记录:

	config set slowlog-log-slower-than 0

然后，执行几个命令。然后你可以检索所有日志，或者最新日志，通过:

	slowlog get
	slowlog get 10

你可以获取 slow log 中记录条数，通过 `slowlog len`

对你执行的每个命令，你可以看到四个参数:

* 一个自增的 id

* 一个 Unix 时间戳，表示命令开始时间

* 执行时间，用微秒表示的, 记录了命令执行总时间

* 命令和它的参数

slow log 在内存中维护，所以在生产环境中执行，即使使用低阈值，应该也没问题。默认，它会跟踪最新 1024 条日志。

## Sort

`sort` 是Redis 最强力命令之一。它允许你对列表，集合，有序集合中的值进行排序 (有序集是依照权重排序的，而不是集合中的成员)。最简单的情况，它允许我们这样:

	rpush users:leto:guesses 5 9 10 2 4 10 19 2
	sort users:leto:guesses

将会返回从低到高顺序排列的值。还有一些更高级例子:

	sadd friends:ghanima leto paul chani jessica alia duncan
	sort friends:ghanima limit 0 3 desc alpha

上面的命令演示了怎么对已排序记录分页 (通过 `limit`)，如何以降序返回结果 (通过 `desc`) 以及如何按照字典序排序而不是按照数值 (通过 `alpha`).

`sort` 真正强力的地方在于它可以对基于引用的对象进行排序。之前我们演示了列表，集合和有序集合是怎样用于引用其他 Redis 对象的。`sort` 命令可以解引用这些关系，并且根据值进行排序。比如，假设我们有一个 bug 跟踪系统，可以让用户查看异常。我们会用一个集合来跟踪被监控的异常:

	sadd watch:leto 12339 1382 338 9338

可能通过 id 对异常进行排序很不错 (默认就是就这样做的)，可是我们也希望能按照严重度来排序的。于是，我们得告诉 Redis 用什么模式来排序。首先，让我们添加一些数据，这样可以让我们看到比较有意义的测试结果:

	set severity:12339 3
	set severity:1382 2
	set severity:338 5
	set severity:9338 4

然后按照 bug 的严重度来排序，从高到低，你可以这样:

	sort watch:leto by severity:* desc

Redis 会将我们指定的模式(用 `by` 标记部分) 中的 `*` ，用我们的列表/集合/有序集的值来替换。然后 Redis 会以此创建 key 名，查询实际值之后再根据结果进行排序。

虽然你可以有上百万的 key 在 Redis 中，但是我觉得上面还是有点乱了。幸好 `sort` 对哈希结构和它的字段也有用。你可以利用哈希结构取代一堆顶级 key:

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

不单事情变简单了，可以根据 `severity` 或 `priority`排序了，我们还可以告诉 `sort` 我们需要取什么值:

	sort watch:leto by bug:*->priority get bug:*->details

和刚才一样有做替换操作，不过 Redis 可以识别 `->` 序列，用它来查找我们哈希结构中指定的字段。我们还加入了 `get` 命令，同样有替换操作和字段查询，用于获取 bug 的详细信息。

对于大集合，`sort` 可能会慢。好消息是 `sort` 的输出结构可以保存起来:

	sort watch:leto by bug:*->priority get bug:*->details store watch_by_priority:leto

`sort` 的 `store` 功能，以及我们已经学过的 `expiration` 命令，可以组成一个非常棒的组合。

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