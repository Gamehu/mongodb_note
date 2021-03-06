<p>通过下图实例，我们也可以更直观的的了解Mongo中的一些概念：</p>
![](http://www.runoob.com/wp-content/uploads/2013/10/Figure-1-Mapping-Table-to-Collection-1.png)
<div class="article-intro" id="content">
			
			<h1>MongoDB  <span class="color_h1">概念解析</span> </h1>

<p>不管我们学习什么数据库都应该学习其中的基础概念，在mongodb中基本的概念是文档、集合、数据库，下面我们挨个介绍。</p>
<p>下表将帮助您更容易理解Mongo中的一些概念：</p>
<table class="reference">
<tbody><tr>
<th>SQL术语/概念</th>
<th>MongoDB术语/概念</th>
<th>解释/说明</th>
</tr>
<tr>
<td>database</td>
<td>database</td>
<td>数据库</td>
</tr>
<tr>
<td>table</td>
<td>collection</td>
<td>数据库表/集合</td>
</tr>
<tr>
<td>row</td>
<td>document</td>
<td>数据记录行/文档</td>
</tr>
<tr>
<td>column</td>
<td>field</td>
<td>数据字段/域</td>
</tr>
<tr>
<td>index</td>
<td>index</td>
<td>索引</td>
</tr>
<tr>
<td>table joins</td>
<td>&nbsp;</td>
<td>表连接,MongoDB不支持</td>
</tr>
<tr>
<td>primary key</td>
<td>primary key</td>
<td>主键,MongoDB自动将_id字段设置为主键</td>
</tr>
</tbody></table>


<hr><h2>数据库</h2>
<p>一个mongodb中可以建立多个数据库。</p>
<p>MongoDB的默认数据库为"db"，该数据库存储在data目录中。</p>
<p>MongoDB的单个实例可以容纳多个独立的数据库，每一个都有自己的集合和权限，不同的数据库也放置在不同的文件中。</p>

<p><strong>"show dbs"</strong> 命令可以显示所有数据的列表。</p>
<pre class="prettyprint prettyprinted" style=""><span class="pln">$ </span><span class="pun">./</span><span class="pln">mongo
</span><span class="typ">MongoDB</span><span class="pln"> shell version</span><span class="pun">:</span><span class="pln"> </span><span class="lit">3.0</span><span class="pun">.</span><span class="lit">6</span><span class="pln">
connecting to</span><span class="pun">:</span><span class="pln"> test
</span><span class="pun">&gt;</span><span class="pln"> show dbs
</span><span class="kwd">local</span><span class="pln">  </span><span class="lit">0.078GB</span><span class="pln">
test   </span><span class="lit">0.078GB</span><span class="pln">
</span><span class="pun">&gt;</span><span class="pln"> </span></pre>
<p>执行 <strong>"db"</strong> 命令可以显示当前数据库对象或集合。</p>
<pre class="prettyprint prettyprinted" style=""><span class="pln">$ </span><span class="pun">./</span><span class="pln">mongo
</span><span class="typ">MongoDB</span><span class="pln"> shell version</span><span class="pun">:</span><span class="pln"> </span><span class="lit">3.0</span><span class="pun">.</span><span class="lit">6</span><span class="pln">
connecting to</span><span class="pun">:</span><span class="pln"> test
</span><span class="pun">&gt;</span><span class="pln"> db
test
</span><span class="pun">&gt;</span><span class="pln"> </span></pre>
<p>运行"use"命令，可以连接到一个指定的数据库。</p>
<pre class="prettyprint prettyprinted" style=""><span class="pun">&gt;</span><span class="pln"> </span><span class="kwd">use</span><span class="pln"> </span><span class="kwd">local</span><span class="pln">
switched to db </span><span class="kwd">local</span><span class="pln">
</span><span class="pun">&gt;</span><span class="pln"> db
</span><span class="kwd">local</span><span class="pln">
</span><span class="pun">&gt;</span><span class="pln"> </span></pre>
<p>以上实例命令中，"local" 是你要链接的数据库。</p>
<p>在下一个章节我们将详细讲解MongoDB中命令的使用。</p>
<p>数据库也通过名字来标识。数据库名可以是满足以下条件的任意UTF-8字符串。</p>
<ul>
<li>不能是空字符串（"")。</li>
<li>不得含有' '（空格)、.、$、/、\和\0 (空宇符)。</li>
<li>应全部小写。</li>
<li>最多64字节。</li>
</ul>
<p>有一些数据库名是保留的，可以直接访问这些有特殊作用的数据库。</p>
<ul><li><strong>admin</strong>：
从权限的角度来看，这是"root"数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器。</li><li><strong>local:</strong>
这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合
</li><li><strong>config</strong>:
当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息。</li></ul>
<hr><h2>文档</h2>
<p>
文档是一个键值(key-value)对(即BSON)。MongoDB 的文档不需要设置相同的字段，并且相同的字段不需要相同的数据类型，这与关系型数据库有很大的区别，也是 MongoDB 非常突出的特点。</p>


<p>一个简单的文档例子如下：</p>
<pre class="prettyprint prettyprinted" style=""><span class="pun">{</span><span class="str">"site"</span><span class="pun">:</span><span class="str">"www.runoob.com"</span><span class="pun">,</span><span class="pln"> </span><span class="str">"name"</span><span class="pun">:</span><span class="str">"菜鸟教程"</span><span class="pun">}</span></pre>
<p>下表列出了 RDBMS 与 MongoDB 对应的术语：</p>
<table class="reference">
<tbody><tr>
<th style="width:50%;">RDBMS</th>
<th>MongoDB</th>
</tr>
<tr>
<td>数据库</td>
<td>数据库</td>
</tr>
<tr>
<td>表格</td>
<td>集合</td>
</tr>
<tr>
<td>行</td>
<td>文档</td>
</tr>
<tr>
<td>列</td>
<td>字段</td>
</tr>
<tr>
<td>表联合</td>
<td>嵌入文档</td>
</tr>
<tr>
<td>主键</td>
<td>主键 (MongoDB 提供了 key  为 _id )</td>
</tr>
<tr>
<th colspan="2" style="text-align:center;">数据库服务和客户端</th>
</tr>
<tr>
<td>Mysqld/Oracle</td>
<td>mongod</td>
</tr>
<tr>
<td>mysql/sqlplus</td>
<td>mongo</td>
</tr>
</tbody></table>
<p>需要注意的是：</p>
<ol>
<li>文档中的键/值对是有序的。</li>
<li>文档中的值不仅可以是在双引号里面的字符串，还可以是其他几种数据类型（甚至可以是整个嵌入的文档)。</li>
<li>MongoDB区分类型和大小写。</li>
<li>MongoDB的文档不能有重复的键。</li>
<li>文档的键是字符串。除了少数例外情况，键可以使用任意UTF-8字符。

</li>
</ol>
<p>文档键命名规范：</p>
<ul>
<li>键不能含有\0 (空字符)。这个字符用来表示键的结尾。</li>
<li>.和$有特别的意义，只有在特定环境下才能使用。</li>
<li>以下划线"_"开头的键是保留的(不是严格要求的)。</li>
</ul>




<hr><h2>集合</h2>

<p>集合就是 MongoDB 文档组，类似于 RDBMS （关系数据库管理系统：Relational Database Management System)中的表格。</p><p>
集合存在于数据库中，集合没有固定的结构，这意味着你在对集合可以插入不同格式和类型的数据，但通常情况下我们插入集合的数据都会有一定的关联性。</p><p>
</p>
<p>比如，我们可以将以下不同数据结构的文档插入到集合中：</p>
<pre class="prettyprint prettyprinted" style=""><span class="pun">{</span><span class="str">"site"</span><span class="pun">:</span><span class="str">"www.baidu.com"</span><span class="pun">}</span><span class="pln">
</span><span class="pun">{</span><span class="str">"site"</span><span class="pun">:</span><span class="str">"www.google.com"</span><span class="pun">,</span><span class="str">"name"</span><span class="pun">:</span><span class="str">"Google"</span><span class="pun">}</span><span class="pln">
</span><span class="pun">{</span><span class="str">"site"</span><span class="pun">:</span><span class="str">"www.runoob.com"</span><span class="pun">,</span><span class="str">"name"</span><span class="pun">:</span><span class="str">"菜鸟教程"</span><span class="pun">,</span><span class="str">"num"</span><span class="pun">:</span><span class="lit">5</span><span class="pun">}</span></pre>
<p>当第一个文档插入时，集合就会被创建。</p>

<h3>合法的集合名</h3>

<ul><li>集合名不能是空字符串""。</li><li>
集合名不能含有\0字符（空字符)，这个字符表示集合名的结尾。</li><li>
集合名不能以"system."开头，这是为系统集合保留的前缀。</li><li>
用户创建的集合名字不能含有保留字符。有些驱动程序的确支持在集合名里面包含，这是因为某些系统生成的集合中包含该字符。除非你要访问这种系统创建的集合，否则千万不要在名字里出现$。　</li></ul><p>如下实例：</p>
<pre class="prettyprint prettyprinted" style=""><span class="pln">db</span><span class="pun">.</span><span class="pln">col</span><span class="pun">.</span><span class="pln">findOne</span><span class="pun">()</span></pre>
<h3>capped collections</h3>
<p>Capped collections 就是固定大小的collection。</p>
<p>它有很高的性能以及队列过期的特性(过期按照插入的顺序). 有点和 "RRD" 概念类似。</p>

<p>Capped collections是高性能自动的维护对象的插入顺序。它非常适合类似记录日志的功能
和标准的collection不同，你必须要显式的创建一个capped collection，
指定一个collection的大小，单位是字节。collection的数据存储空间值提前分配的。</p>
<p></p>要注意的是指定的存储大小包含了数据库的头信息。<p></p>
<pre class="prettyprint prettyprinted" style=""><span class="pln">db</span><span class="pun">.</span><span class="pln">createCollection</span><span class="pun">(</span><span class="str">"mycoll"</span><span class="pun">,</span><span class="pln"> </span><span class="pun">{</span><span class="pln">capped</span><span class="pun">:</span><span class="kwd">true</span><span class="pun">,</span><span class="pln"> size</span><span class="pun">:</span><span class="lit">100000</span><span class="pun">})</span></pre>
<ul>
	<li>在capped collection中，你能添加新的对象。</li>
	<li>能进行更新，然而，对象不会增加存储空间。如果增加，更新就会失败 。</li>
	<li>数据库不允许进行删除。使用drop()方法删除collection所有的行。</li>
	<li>注意: 删除之后，你必须显式的重新创建这个collection。</li>
	<li>在32bit机器中，capped collection最大存储为1e9( 1X10<sup>9</sup>)个字节。</li>
</ul>
<hr><h2>元数据</h2>
<p>数据库的信息是存储在集合中。它们使用了系统的命名空间：</p>
<pre class="prettyprint prettyprinted" style=""><span class="pln">dbname</span><span class="pun">.</span><span class="pln">system</span><span class="pun">.*</span></pre>
<p>在MongoDB数据库中名字空间 &lt;dbname&gt;.system.* 是包含多种系统信息的特殊集合(Collection)，如下:</p>
<table class="reference">
<tbody>
<tr>
<th>集合命名空间</th>
<th>描述</th>
</tr>
<tr>
<td>dbname.system.namespaces</td>
<td>列出所有名字空间。</td>
</tr>
<tr>
<td>dbname.system.indexes</td>
<td>列出所有索引。</td>
</tr>
<tr>
<td>dbname.system.profile</td>
<td>包含数据库概要(profile)信息。</td>
</tr>
<tr>
<td>dbname.system.users</td>
<td>列出所有可访问数据库的用户。</td>
</tr>
<tr>
<td>dbname.local.sources</td>
<td>包含复制对端（slave）的服务器信息和状态。</td>
</tr>
</tbody>
</table>
<p>对于修改系统集合中的对象有如下限制。</p>
<p>在{{system.indexes}}插入数据，可以创建索引。但除此之外该表信息是不可变的(特殊的drop index命令将自动更新相关信息)。 </p>
<p>{{system.users}}是可修改的。 {{system.profile}}是可删除的。</p>
<hr>
<h2>MongoDB 数据类型</h2>
<p>下表为MongoDB中常用的几种数据类型。</p>
<table class="reference">
<tbody><tr>
<th>数据类型</th>
<th>描述</th>
</tr>
<tr><td>String</td><td>字符串。存储数据常用的数据类型。在 MongoDB 中，UTF-8 编码的字符串才是合法的。   </td></tr>
<tr><td>Integer</td><td>整型数值。用于存储数值。根据你所采用的服务器，可分为 32 位或 64 位。  </td></tr>
<tr><td>Boolean</td><td>布尔值。用于存储布尔值（真/假）。  </td></tr>
<tr><td>Double</td><td>双精度浮点值。用于存储浮点值。  </td></tr>
<tr><td>Min/Max keys</td><td>将一个值与 BSON（二进制的 JSON）元素的最低值和最高值相对比。  </td></tr>
<tr><td>Arrays</td><td>用于将数组或列表或多个值存储为一个键。  </td></tr>
<tr><td>Timestamp</td><td>时间戳。记录文档修改或添加的具体时间。  </td></tr>
<tr><td>Object</td><td>用于内嵌文档。  </td></tr>
<tr><td>Null</td><td>用于创建空值。  </td></tr>
<tr><td>Symbol</td><td>符号。该数据类型基本上等同于字符串类型，但不同的是，它一般用于采用特殊符号类型的语言。</td></tr>
<tr><td>Date</td><td>日期时间。用 UNIX 时间格式来存储当前日期或时间。你可以指定自己的日期时间：创建 Date 对象，传入年月日信息。  </td></tr>
<tr><td>Object ID</td><td>对象 ID。用于创建文档的 ID。  </td></tr>
<tr><td>Binary Data</td><td>二进制数据。用于存储二进制数据。</td></tr>
<tr><td>Code</td><td>代码类型。用于在文档中存储 JavaScript 代码。</td></tr>
<tr><td>Regular expression</td><td>正则表达式类型。用于存储正则表达式。</td></tr>
</tbody></table>			
			
			</div>
