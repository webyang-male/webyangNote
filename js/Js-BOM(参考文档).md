<!DOCTYPE html><html><head><title>19-第十九章 BOM</title><meta charset='utf-8'><link href='https://dn-maxiang.qbox.me/res-min/themes/marxico.css' rel='stylesheet'><style>
.note-content  {font-family: 'Helvetica Neue', Arial, 'Hiragino Sans GB', STHeiti, 'Microsoft YaHei', 'WenQuanYi Micro Hei', SimSun, Song, sans-serif;}


<h1 id="19-第十九章-bom"> BOM</h1>

<blockquote>
  <p><code>浏览器对象模型</code> (BOM) 使 JavaScript 有能力与浏览器“对话”。</p>
</blockquote>

<hr>

<p><code>Window 对象</code> 它表示<code>浏览器窗口</code>。 <br>
所有 JavaScript <code>全局对象</code>、<code>函数</code>以及<code>变量</code>均自动成为 window 对象的成员。 <br>
<code>全局变量</code>是 window 对象的<code>属性</code>。 <br>
<code>全局函数</code>是 window 对象的<code>方法</code>。 <br>
 HTML DOM 的 <code>document</code> 也是 window 对象的属性之一</p>



<pre class="prettyprint with-line-number hljs-dark"><code class="hljs coffeescript"><div class="hljs-line"><span class="hljs-comment line-number">1.</span><span class="hljs-built_in">window</span>.<span class="hljs-built_in">document</span>.getElementById(<span class="hljs-string">"header"</span>);
</div></code></pre>

<hr>



<h2 id="一-window-对象属性">一 Window  对象属性</h2>



<h4 id="1-document-document-对象">1 ）<code>document</code>    Document 对象</h4>



<h4 id="2-location-浏览器地址信息">2 ）<code>location</code>      浏览器地址信息</h4>

<p><code>Location</code> <strong>对象属性</strong>:</p>

<table>
<thead>
<tr>
  <th align="left">对象属性</th>
  <th align="right">描述</th>
</tr>
</thead>
<tbody><tr>
  <td align="left">window.location.<code>href</code></td>
  <td align="right">=  设置或返回完整的 URL。</td>
</tr>
<tr>
  <td align="left">window.location.<code>search</code></td>
  <td align="right">= 设置或返回 url<code>?</code>,?+后面的内容。</td>
</tr>
<tr>
  <td align="left">window.location. <code>hash</code></td>
  <td align="right">= 设置或返回 url<code>#</code>后面的内容</td>
</tr>
<tr>
  <td align="left">window.location.<code>port</code></td>
  <td align="right">设置或返回当前 URL 的端口号。</td>
</tr>
<tr>
  <td align="left">window.location.<code>hostname</code></td>
  <td align="right">设置或返回当前 URL 的<code>主机名</code>。</td>
</tr>
<tr>
  <td align="left">window.location.<code>host</code></td>
  <td align="right">设置或返回<code>主机名</code>和当前 URL 的<code>端口号</code></td>
</tr>
<tr>
  <td align="left">window.location.<code>pathname</code></td>
  <td align="right">设置或返回当前 URL 的<code>路径部分</code></td>
</tr>
<tr>
  <td align="left">window.location.<code>protocol</code></td>
  <td align="right">设置或返回当前 URL 的<code>协议</code></td>
</tr>
</tbody></table>


<hr>



<h4 id="3-history-历史记录">3 ）<code>history</code>      历史记录</h4>

<blockquote>
  <p>History 对象包含用户（在浏览器窗口中）访问过的 URL。</p>
</blockquote>

<p><strong>属性</strong> <br>
<code>length</code>    返回浏览器历史列表中的 URL 数量。  <br>
<strong>方法</strong> <br>
<code>back()</code>            加载 history 列表中的前一个 URL。 <br>
<code>forward()</code>   加载 history 列表中的下一个 URL。 <br>
<code>go()</code>              加载 history 列表中的某个具体页面。 <br>
下面一行代码执行的操作与单击两次后退按钮执行的操作一样：</p>



<pre class="prettyprint with-line-number hljs-dark"><code class="hljs vim"><div class="hljs-line"><span class="hljs-comment line-number">1.</span><span class="hljs-keyword">history</span>.<span class="hljs-keyword">go</span>(-<span class="hljs-number">2</span>)
</div></code></pre>

<hr>



<h4 id="4-navigator-对-navigator-对象的只读引用">4 ）<code>Navigator</code>  对 Navigator 对象的只读引用</h4>

<pre class="prettyprint with-line-number hljs-dark"><code class="hljs autohotkey"><div class="hljs-line"><span class="hljs-comment line-number">1.</span>window.`navigator.userAgent` 浏览器信息
</div></code></pre>



<pre class="prettyprint with-line-number hljs-dark"><code class="hljs xquery"><div class="hljs-line"><span class="hljs-comment line-number">1.</span>
</div><div class="hljs-line"><span class="hljs-comment line-number">2.</span>//alert( <span class="hljs-keyword">window</span>.navigator.userAgent )
</div><div class="hljs-line"><span class="hljs-comment line-number">3.</span><span class="hljs-keyword">if</span> ( <span class="hljs-keyword">window</span>.navigator.userAgent.indexOf(<span class="hljs-string">'MSIE'</span>) != -<span class="hljs-number">1</span> ) {
</div><div class="hljs-line"><span class="hljs-comment line-number">4.</span>    alert(<span class="hljs-string">'我是ie'</span>);
</div><div class="hljs-line"><span class="hljs-comment line-number">5.</span>    } <span class="hljs-keyword">else</span> {
</div><div class="hljs-line"><span class="hljs-comment line-number">6.</span>    alert(<span class="hljs-string">'我不是ie'</span>);
</div><div class="hljs-line"><span class="hljs-comment line-number">7.</span>}
</div></code></pre>

<hr>



<h2 id="二-window-对象方法">二 Window 对象方法</h2>

<hr>



<h4 id="01-open-打开一个新的浏览器窗口或查找一个已命名的窗口">01 ）  <code>open()</code>  打开一个新的浏览器窗口或查找一个已命名的窗口。</h4>



<pre class="prettyprint with-line-number hljs-dark"><code class="hljs stata"><div class="hljs-line"><span class="hljs-comment line-number">1.</span>  <span class="hljs-keyword">window</span>.<span class="hljs-keyword">open</span>(url,target)
</div><div class="hljs-line"><span class="hljs-comment line-number">2.</span><span class="hljs-keyword">open</span>(地址默认是空白页面，打开方式默认新窗口) 打开一个新窗口
</div><div class="hljs-line"><span class="hljs-comment line-number">3.</span>          <span class="hljs-keyword">window</span>.<span class="hljs-keyword">open</span>('http:<span class="hljs-comment">//www.baidu.com', '_self');</span>
</div><div class="hljs-line"><span class="hljs-comment line-number">4.</span>                <span class="hljs-keyword">var</span> opener = <span class="hljs-keyword">window</span>.<span class="hljs-keyword">open</span>();<span class="hljs-comment">//返回值 返回的新开页面的window对象</span>
</div><div class="hljs-line"><span class="hljs-comment line-number">5.</span>                opener.document.body.style.background = 'red';
</div><div class="hljs-line"><span class="hljs-comment line-number">6.</span>        <span class="hljs-keyword">window</span>.<span class="hljs-keyword">close</span>()
</div><div class="hljs-line"><span class="hljs-comment line-number">7.</span>        opener.<span class="hljs-keyword">close</span>();<span class="hljs-comment">//可以通过关闭用window.open方法打开的窗口</span>
</div></code></pre>

<hr>

<h4 id="02-close-关闭浏览器窗口">02 ）  <code>close()</code> 关闭浏览器窗口。</h4>

<hr>

<h4 id="07scrollto-把内容滚动到指定的坐标">07）<code>scrollTo()</code> 把内容滚动到指定的坐标。</h4>

<hr>

<pre class="prettyprint with-line-number hljs-dark"><code class="hljs javascript"><div class="hljs-line"><span class="hljs-comment line-number">1.</span><span class="hljs-built_in">document</span>.onclick = <span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params"></span>)</span>{
</div><div class="hljs-line"><span class="hljs-comment line-number">2.</span>            <span class="hljs-built_in">window</span>.scrollTo(<span class="hljs-number">0</span>,<span class="hljs-number">500</span>);
</div><div class="hljs-line"><span class="hljs-comment line-number">3.</span>        }
</div></code></pre>

<hr>



<h4 id="8-scrollby">8 ）<code>scrollBy()</code></h4>

<blockquote>
  <p>scrollBy(xnum,ynum)  指定的像素值来滚动内容。<code>不带px</code> <br>
  <code>xnum</code> 必需。把文档向右滚动的像素数 。 <br>
  <code>ynum</code> 必需。把文档向下滚动的像素数。</p>
</blockquote>



<pre class="prettyprint with-line-number hljs-dark"><code class="hljs javascript"><div class="hljs-line"><span class="hljs-comment line-number">1.</span><span class="hljs-built_in">document</span>.onclick = <span class="hljs-function"><span class="hljs-keyword">function</span>(<span class="hljs-params"></span>)</span>{
</div><div class="hljs-line"><span class="hljs-comment line-number">2.</span>            <span class="hljs-built_in">window</span>.scrollBy(<span class="hljs-number">0</span>,<span class="hljs-number">500</span>);
</div><div class="hljs-line"><span class="hljs-comment line-number">3.</span>        }
</div></code></pre>

<hr>



<h4 id="9-alert-内容-警告框">9 ）<code>alert</code>( 内容 <code>)</code> 警告框</h4>

<blockquote>
  <p><code>alert</code>( 内容 <code>)``警告框</code>经常用于弹出警告信息，无返回值</p>
</blockquote>

<hr>



<h4 id="10-confirm文本-确认框">10 ）<code>confirm(</code>“文本”<code>)</code>  确认框</h4>

<hr>

<blockquote>
  <p><code>confirm(</code>“文本”<code>)``确认框</code>用于使用户可以验证或者接受某些信息。 <br>
  如果用户点击确认，那么返回值为 <code>true</code>。如果用户点击取消，那么返回值为 <code>false</code>。</p>
</blockquote>

<hr>



<h4 id="11-prompt文本默认值">11 ）<code>prompt(</code>“文本”,”默认值”<code>)</code></h4>

<blockquote>
  <p><code>prompt(</code>“提示”,”默认值”<code>)</code>提示框经常用于提示用户在进入页面前输入某个值。 <br>
  如果用户点击确认，那么返回<code>输入的值</code>。如果用户点击取消，那么返回值为 <code>null</code>。</p>
</blockquote>



<pre class="prettyprint with-line-number hljs-dark"><code class="hljs javascript"><div class="hljs-line"><span class="hljs-comment line-number">1.</span><span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-title">disp_prompt</span>(<span class="hljs-params"></span>)
</span></div><div class="hljs-line"><span class="hljs-comment line-number">2.</span>  {
</div><div class="hljs-line"><span class="hljs-comment line-number">3.</span>  <span class="hljs-keyword">var</span> name=prompt(<span class="hljs-string">"请输入您的名字"</span>,<span class="hljs-string">"Bill"</span>)
</div><div class="hljs-line"><span class="hljs-comment line-number">4.</span>  <span class="hljs-keyword">if</span> (name!=<span class="hljs-literal">null</span> &amp;&amp; name!=<span class="hljs-string">""</span>)
</div><div class="hljs-line"><span class="hljs-comment line-number">5.</span>    {
</div><div class="hljs-line"><span class="hljs-comment line-number">6.</span>    <span class="hljs-built_in">document</span>.write(<span class="hljs-string">"你好！"</span> + name + <span class="hljs-string">" 今天过得怎么样？"</span>)
</div><div class="hljs-line"><span class="hljs-comment line-number">7.</span>    }
</div><div class="hljs-line"><span class="hljs-comment line-number">8.</span>  }
</div></code></pre>

<hr>



<h2 id="二-window对象常用事件">二 window对象常用事件</h2>

<hr>

<ul><li><code>onload</code> 文档加载完毕</li>
<li><code>onscroll</code> 滚动的时候</li>
<li><code>onresize</code> 调整尺寸的时候</li>
</ul></div></body></html>