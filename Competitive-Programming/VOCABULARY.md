# 📚 AtCoder & IT Interview 三位一体全场景词汇大地图

此表为长期参考手册，涵盖了算法竞赛（ABC/ARC）、项目开发、以及日本 IT 企业面试的核心术语。

---

## 1. 基础语法与控制流 (Basic Syntax)
| 日本語 (读音) | 英语 (English) | 中文含义 | 备注 |
| :--- | :--- | :--- | :--- |
| 変数 (へんすう) | Variable | 变量 | |
| 定数 (ていすう) | Constant | 常量 | const |
| 型 (かた) | Type | 类型 | int, double, char |
| 宣言 (せんげん) | Declaration | 声明/定义 | 变量定义 |
| 代入 (だいにゅう) | Assignment | 赋值 | a = b |
| 初期化 (しょきか) | Initialization | 初始化 | 赋初值 |
| 条件分岐 (ぶんき) | Branching | 条件分支 | if / switch |
| 反復 / ループ | Iteration / Loop | 循环 | for / while |
| 関数 (かんすう) | Function | 函数 | |
| 引数 (ひきすう) | Argument / Parameter | 参数 | 函数输入 |
| 戻り値 (もどりち) | Return Value | 返回值 | 函数输出 |

## 2. 容器与数据结构 (Data Structures)
| 日本語 (读音) | 英语 (English) | 中文含义 | 场景 |
| :--- | :--- | :--- | :--- |
| 配列 (はいれつ) | Array | 数组 | 固定长度 |
| 動的配列 | Vector | 动态数组 | 自动扩容 |
| 連結リスト | Linked List | 链表 | 插入删除快 |
| 双方向リスト | Doubly Linked List | 双向链表 | 可双向遍历 |
| スタック (LIFO) | Stack | 栈 | 后进先出 |
| キュー (FIFO) | Queue | 队列 | 先进先出 |
| 優先度付きキュー | Priority Queue | 优先队列 | 堆实现 |
| 連想配列 (れんそう) | Map / Dictionary | 映射/哈希表 | 键值对 |
| 集合 (しゅうごう) | Set | 集合 | 去重/排序 |
| 木 (き) / 枝 (えだ) | Tree / Branch | 树 / 分支 | 树形结构 |
| 根 (ね) / 葉 (は) | Root / Leaf | 根 / 叶子 | 树的层级 |
| 親 (おや) / 子 (こ) | Parent / Child | 父节点 / 子节点 | 树的关系 |
| 深さ (ふかさ) | Depth | 深度 | 离根距离 |
| 高さ (たかさ) | Height | 高度 | 离叶子距离 |

## 3. 数学与位运算 (Math & Bitwise)
| 日本語 (读音) | 英语 (English) | 中文含义 | 技巧 |
| :--- | :--- | :--- | :--- |
| 算術演算 (さんじゅつ) | Arithmetic | 算术运算 | +, -, *, / |
| 剰余 (じょうよ) | Modulo | 取余 | % |
| 商 (しょう) | Quotient | 商 | / (整数) |
| 絶対値 (ぜったいち) | Absolute Value | 绝对值 | abs() |
| 切り捨て (きりすて) | Floor / Truncate | 向下取整 | floor() |
| 切り上げ (きりあげ) | Ceil | 向上取整 | ceil() |
| 四捨五入 | Rounding | 四舍五入 | round() |
| 素数 (そすう) | Prime Number | 质数 | 筛选法常见 |
| 偶数 / 奇数 | Even / Odd | 偶数 / 奇数 | |
| 最大公約数 (GCD) | GCD | 最大公约数 | gcd(a, b) |
| 最小公倍数 (LCM) | LCM | 最小公倍数 | lcm(a, b) |
| ビット演算 | Bitwise | 位运算 | &, \|, ^ |
| 左シフト / 右シフト | Left / Right Shift | 左移 / 右移 | <<, >> |
| 進数 (しんすう) | Base / Radix | 进制 | 10进制, 2进制 |

## 4. 搜索与核心算法 (Algorithms)
| 日本語 (读音) | 英语 (English) | 中文含义 | 重点 |
| :--- | :--- | :--- | :--- |
| 線形探索 | Linear Search | 线性搜索 | O(N) |
| 二分探索 (にぶん) | Binary Search | 二分搜索 | O(log N) |
| 全探索 (ぜんたんさく) | Full Search | 暴力搜索 | 尝试所有方案 |
| 深さ優先探索 (DFS) | DFS | 深度优先搜索 | 递归/栈 |
| 幅優先探索 (BFS) | BFS | 广度优先搜索 | 队列 |
| 累積和 (るいせきわ) | Prefix Sum | 前缀和 | 快速区间查询 |
| 貪欲法 (どんよく) | Greedy Algorithm | 贪心算法 | 局部最优 |
| 動的計画法 (DP) | DP | 动态规划 | 状态转移 |
| 尺取り法 | Two Pointers | 双指针/尺蠖法 | 滑动窗口 |
| いもす法 | Imos Method | 差分法 | 区间批量加法 |
| 半分全列挙 | Meet-in-the-middle | 折半搜索 | 降复杂度 |
| 座標圧縮 (ざひょう) | Coordinate Compression | 坐标压缩 | 离散化 |

## 5. 图论进阶 (Graph Theory)
| 日本語 (读音) | 英语 (English) | 中文含义 | 算法 |
| :--- | :--- | :--- | :--- |
| 頂点 (ちょうてん) | Vertex / Node | 顶点 | 节点 |
| 辺 (へん) | Edge | 边 | 连接线 |
| 重み (おもみ) | Weight | 权重 | 路径长度 |
| 経路 (けいろ) | Path | 路径 | 走路序列 |
| 最短経路 | Shortest Path | 最短路径 | Dijkstra / Bellman |
| 隣接リスト | Adjacency List | 邻接表 | 存图首选 |
| 隣接行列 | Adjacency Matrix | 邻接矩阵 | 二维数组存图 |
| 有向グラフ | Directed Graph | 有向图 | 有箭头 |
| 無向グラフ | Undirected Graph | 无向图 | 无箭头 |
| 連結 (れんけつ) | Connected | 连通 | 有路径可达 |
| サイクル / 閉路 | Cycle | 环 | 回到原点 |
| 最小全域木 (MST) | MST | 最小生成树 | Kruskal / Prim |
| 強連結成分 (SCC) | SCC | 强连通分量 | 有向图闭环 |

## 6. 字符串处理 (Strings)
| 日本語 (读音) | 英语 (English) | 中文含义 | 备注 |
| :--- | :--- | :--- | :--- |
| 文字列 (もじれつ) | String | 字符串 | |
| 文字 (もじ) | Character | 字符 | char |
| 長さ (ながさ) | Length / Size | 长度 | s.length() |
| 連結 (れんけつ) | Concatenation | 拼接 | s1 + s2 |
| 部分文字列 | Substring | 子串 | s.substr() |
| 接頭辞 (せっとう) | Prefix | 前缀 | 开头部分 |
| 接尾辞 (せつび) | Suffix | 后缀 | 结尾部分 |
| 反転 (はんてん) | Reverse | 翻转 | reverse(s) |
| 置換 (ちかん) | Replace | 替换 | 换字符 |
| 回文 (かいぶん) | Palindrome | 回文 | 正读反读一样 |

## 7. 复杂度与性能 (Complexity & CS)
| 日本語 (读音) | 英语 (English) | 中文含义 | 评价指标 |
| :--- | :--- | :--- | :--- |
| 計算量 (けいさん) | Complexity | 复杂度 | 时间/空间 |
| 定数 (ていすう) | Constant | 常数 | O(1) |
| 対数 (たいすう) | Logarithmic | 对数 | O(log N) |
| 線形 (せんけい) | Linear | 线性 | O(N) |
| 多項式 (たこう) | Polynomial | 多项式 | O(N^k) |
| 指数 (しすう) | Exponential | 指数 | O(2^N) |
| 階乗 (かいじょう) | Factorial | 阶乘 | O(N!) |
| メモリ制限 | Memory Limit | 内存限制 | 1024MB |
| 実行時間制限 | Time Limit | 时间限制 | 2.0s |
| オーバーフロー | Overflow | 溢出 | 注意 long long |
| 境界値 (きょうかい) | Boundary Value | 边界值 | 考虑极端情况 |

## 8. 开发与工程实践 (Engineering)
| 日本語 (读音) | 英语 (English) | 中文含义 | 面试常考 |
| :--- | :--- | :--- | :--- |
| 実装 (じっそう) | Implementation | 实现 | 码代码 |
| 設計 (せっけい) | Design | 设计 | 系统架构 |
| 仕様書 (しよう) | Specification | 规格说明书 | 需求文档 |
| 保守 (ほしゅ) | Maintenance | 维护 | 长期管理 |
| 運用 (うんよう) | Operation | 运维 | 线上跑程序 |
| 拡張性 (かくちょう) | Scalability | 可扩展性 | 加功能方便 |
| 可読性 (かどくせい) | Readability | 易读性 | 代码整洁度 |
| 抽象化 (ちゅうしょう) | Abstraction | 抽象化 | 隐藏细节 |
| カプセル化 | Encapsulation | 封装 | 面向对象 |
| 継承 (けいしょう) | Inheritance | 继承 | OOP 核心 |

## 9. 职场与简历 (Career & Resume)
| 日本語 (读音) | 英语 (English) | 中文含义 | 求职用语 |
| :--- | :--- | :--- | :--- |
| 自己研鑽 (けんさん) | Self-improvement | 自我钻研 | 形容你刷题 |
| 強み (つよみ) | Strength | 优势/特长 | 你的闪光点 |
| 弱み (よわみ) | Weakness | 缺点 | |
| 志望動機 | Motivation | 动机 | 为什么想来这 |
| 自己PR (ピーアール) | Self-promotion | 自我推荐 | 夸自己 |
| 選考 (せんこう) | Selection | 选拔/面试流程 | |
| 内定 (ないてい) | Job Offer | 录用通知 | 最终目标 |
| 现场 (げんば) | Field / On-site | 现场/一线 | 实际开发环境 |
| 挫折 (ざせつ) | Setback / Failure | 挫折 | 遇到的困难 |

## 10. 竞赛常见判定结果 (Judge Status)
| 略称 | 判定 (日本語) | 中文含义 | 对应原因 |
| :--- | :--- | :--- | :--- |
| **AC** | 正解 (せいかい) | 正确 | 全部样例通过 |
| **WA** | 不正解 | 答案错误 | 逻辑有误 |
| **TLE** | 実行時間超過 | 时间超限 | 复杂度太高 |
| **MLE** | メモリ制限超過 | 内存超限 | 数组开太大 |
| **RE** | 実行時エラー | 运行时错误 | 数组越界/除零 |
| **CE** | コンパイルエラー | 编译错误 | 语法写错 |
