<div class="article-intro">
					<p>Linux 查看端口占用情况可以使用 <strong>lsof</strong> 和 <strong>netstat</strong> 命令。</p>

<hr>
<h2>lsof</h2>
<p>lsof(list open files)是一个列出当前系统打开文件的工具。</p>
<p>lsof  查看端口占用语法格式：</p>

<pre class="prettyprint prettyprinted" style=""><span class="pln">lsof </span><span class="pun">-</span><span class="pln">i</span><span class="pun">:端口号</span></pre>

<h3>实例</h3>

<p>查看服务器  8000 端口的占用情况：</p>

<pre class="prettyprint prettyprinted" style=""><span class="com"># lsof -i:8000</span><span class="pln">
COMMAND   PID USER   FD   TYPE   DEVICE SIZE</span><span class="pun">/</span><span class="pln">OFF NODE NAME
nodejs  </span><span class="lit">26993</span><span class="pln"> root   </span><span class="lit">10u</span><span class="pln">  </span><span class="typ">IPv4</span><span class="pln"> </span><span class="lit">37999514</span><span class="pln">      </span><span class="lit">0t0</span><span class="pln">  TCP </span><span class="pun">*:</span><span class="lit">8000</span><span class="pln"> </span><span class="pun">(</span><span class="pln">LISTEN</span><span class="pun">)</span></pre>

<p>可以看到 8000 端口已经被轻 nodejs 服务占用。</p>

<p>lsof -i 需要 root 用户的权限来执行，如下图：</p>
<p><img src="http://www.runoob.com/wp-content/uploads/2018/09/lsof.png"></p>

<p>更多 lsof 的命令如下：</p>
<pre class="prettyprint prettyprinted" style=""><span class="pln">lsof </span><span class="pun">-</span><span class="pln">i</span><span class="pun">:</span><span class="lit">8080</span><span class="pun">：查看</span><span class="lit">8080</span><span class="pun">端口占用</span><span class="pln">
lsof abc</span><span class="pun">.</span><span class="pln">txt</span><span class="pun">：显示开启文件</span><span class="pln">abc</span><span class="pun">.</span><span class="pln">txt</span><span class="pun">的进程</span><span class="pln">
lsof </span><span class="pun">-</span><span class="pln">c abc</span><span class="pun">：显示</span><span class="pln">abc</span><span class="pun">进程现在打开的文件</span><span class="pln">
lsof </span><span class="pun">-</span><span class="pln">c </span><span class="pun">-</span><span class="pln">p </span><span class="lit">1234</span><span class="pun">：列出进程号为</span><span class="lit">1234</span><span class="pun">的进程所打开的文件</span><span class="pln">
lsof </span><span class="pun">-</span><span class="pln">g gid</span><span class="pun">：显示归属</span><span class="pln">gid</span><span class="pun">的进程情况</span><span class="pln">
lsof </span><span class="pun">+</span><span class="pln">d </span><span class="pun">/</span><span class="pln">usr</span><span class="pun">/</span><span class="kwd">local</span><span class="pun">/：显示目录下被进程开启的文件</span><span class="pln">
lsof </span><span class="pun">+</span><span class="pln">D </span><span class="pun">/</span><span class="pln">usr</span><span class="pun">/</span><span class="kwd">local</span><span class="pun">/：同上，但是会搜索目录下的目录，时间较长</span><span class="pln">
lsof </span><span class="pun">-</span><span class="pln">d </span><span class="lit">4</span><span class="pun">：显示使用</span><span class="pln">fd</span><span class="pun">为</span><span class="lit">4</span><span class="pun">的进程</span><span class="pln">
lsof </span><span class="pun">-</span><span class="pln">i </span><span class="pun">-</span><span class="pln">U</span><span class="pun">：显示所有打开的端口和</span><span class="pln">UNIX domain</span><span class="pun">文件</span></pre>
<hr>
<h2>netstat</h2>
<p><span class="marked">netstat -tunlp</span> 用于显示 tcp，udp 的端口和进程等相关情况。</p>


<p>netstat  查看端口占用语法格式：</p>

<pre class="prettyprint prettyprinted" style=""><span class="pln">netstat </span><span class="pun">-</span><span class="pln">tunlp </span><span class="pun">|</span><span class="pln"> grep </span><span class="pun">端口号</span></pre>
<ul><li>
-t (tcp) 仅显示tcp相关选项</li><li>
-u (udp)仅显示udp相关选项</li><li>
-n 拒绝显示别名，能显示数字的全部转化为数字</li><li>
-l 仅列出在Listen(监听)的服务状态</li><li>
-p 显示建立相关链接的程序名</li></ul>
例如查看 8000 端口的情况，使用以下命令：
<pre class="prettyprint prettyprinted" style=""><span class="com"># netstat -tunlp | grep 8000</span><span class="pln">
tcp        </span><span class="lit">0</span><span class="pln">      </span><span class="lit">0</span><span class="pln"> </span><span class="lit">0.0</span><span class="pun">.</span><span class="lit">0.0</span><span class="pun">:</span><span class="lit">8000</span><span class="pln">            </span><span class="lit">0.0</span><span class="pun">.</span><span class="lit">0.0</span><span class="pun">:*</span><span class="pln">               LISTEN      </span><span class="lit">26993</span><span class="pun">/</span><span class="pln">nodejs   </span></pre>


<p>更多命令：</p>
<pre class="prettyprint prettyprinted" style=""><span class="pln">netstat </span><span class="pun">-</span><span class="pln">ntlp   </span><span class="com">//查看当前所有tcp端口</span><span class="pln">
netstat </span><span class="pun">-</span><span class="pln">ntulp </span><span class="pun">|</span><span class="pln"> grep </span><span class="lit">80</span><span class="pln">   </span><span class="com">//查看所有80端口使用情况</span><span class="pln">
netstat </span><span class="pun">-</span><span class="pln">ntulp </span><span class="pun">|</span><span class="pln"> grep </span><span class="lit">3306</span><span class="pln">   </span><span class="com">//查看所有3306端口使用情况</span></pre>


<hr>
<h2>kill</h2>



<p>在查到端口占用的进程后，如果你要杀掉对应的进程可以使用 kill 命令：</p>


<pre class="prettyprint prettyprinted" style=""><span class="pln">kill </span><span class="pun">-</span><span class="lit">9</span><span class="pln"> PID</span></pre>
<p>如上实例，我们看到 8000 端口对应的 PID 为 26993，使用以下命令杀死进程：</p>
<pre class="prettyprint prettyprinted" style=""><span class="pln">kill </span><span class="pun">-</span><span class="lit">9</span><span class="pln"> </span><span class="lit">26993</span></pre>				</div>