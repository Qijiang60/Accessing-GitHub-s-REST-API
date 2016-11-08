<p>下午更新了 <a rel="nofollow" href="http://cirru.org">http://cirru.org</a> , 整个页面的文档都是通过抓 GitHub README 生成的,</p>

<h4>已经失效的直接 GET 的方法</h4>

<p>抓 READMD 原来是有个简单的思路, 通过 HTTP 请求直接拿 raw 的文件,<br>
比如 cirru.org 项目的 README 文件, 原来的网址是这样的:<br><a rel="nofollow" href="https://github.com/Cirru/cirru.org/blob/master/README.md">https://github.com/Cirru/cirru.org/blob/master/README.md</a><br>
页面上代码右上角又个 raw 的链接, 可以获取纯文本的内容:<br><a rel="nofollow" href="https://raw.githubusercontent.com/Cirru/cirru.org/master/README.md">https://raw.githubusercontent.com/Cirru/cirru.org/master/README.md</a><br>
不过这个链接做了跨域限制, 不能直接用...</p>

<p>我记得早先的网址, 其实更短一些, 是这样的:<br><a rel="nofollow" href="https://raw.github.com/Cirru/cirru.org/master/README.md">https://raw.github.com/Cirru/cirru.org/master/README.md</a><br>
这个网址的特别之处在于, 通过去掉两个字母可以获得允许直接调用的网址:<br><a rel="nofollow" href="http://rawgithub.com/Cirru/cirru.org/master/README.md">http://rawgithub.com/Cirru/cirru.org/master/README.md</a><br>
不知什么缘故, 这个网址现在是个重定向, 内容有拿不到了</p>

<h4>通过 GitHub API</h4>

<p>通过查文档, 确认 GitHub 是有获取 README 的 API 的:<br><a rel="nofollow" href="http://developer.github.com/v3/repos/contents/#get-the-readme">http://developer.github.com/v3/repos/contents/#get-the-readme</a></p>

<pre><code>GET /repos/:owner/:repo/readme
</code></pre>

<p>不过网址没明确写, 搜索加猜测找到网站是这样的:<br><a rel="nofollow" href="https://api.github.com/repos/Cirru/cirru.org/readme">https://api.github.com/repos/Cirru/cirru.org/readme</a></p>

<p>从返回结果看, 里边没有 README 的文本内容, 而是包含一段 base64 字符,<br>
搜索可以知道 JS 里可以通过 <code>atob</code> 函数进行解码:<br><a rel="nofollow" href="https://developer.mozilla.org/en-US/docs/Web/API/Window.atob">https://developer.mozilla.org/en-US/docs/Web/API/Window.atob</a><br>
于是, 就能够通过获取 README 真实的文本内容了.</p>

<h4>两个注意的地方</h4>

<p>GitHub API 单个 IP 的访问数量存在限制, 需要通过注册应用来解决:<br><a rel="nofollow" href="https://github.com/settings/applications/">https://github.com/settings/applications/</a><br>
具体做法是在请求的 url 后面加上注册好的 key<br><a rel="nofollow" href="http://developer.github.com/v3/#oauth2-keysecret">http://developer.github.com/v3/#oauth2-keysecret</a></p>

<pre><code>$ curl 'https://api.github.com/users/whatever?client_id=xxxx&amp;client_secret=yyyy'
</code></pre>

<p>(我图方便直接在网页上使用了, 可能存在问题...)</p>

<p>另一个是我使用中发现非 ASCII 的字符, 比如国际音标, 显示为乱码<br>
估计是 base64 编码过程的问题, 我没想到好的办法..<br>
中文没有测试, 估计也会出现问题...<br>
我发了对应的问题,: <a rel="nofollow" href="http://segmentfault.com/q/1010000000451621">http://segmentfault.com/q/1010000000451621</a></p>

                </div>
