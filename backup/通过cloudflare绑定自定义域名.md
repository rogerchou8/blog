<html>
<body>
<!--StartFragment--><html><head></head><body><p>要将 <strong>ngrok 地址 (<code inline="">https://xxxxxx.ngrok-free.app</code>)</strong> 绑定到 <strong>Cloudflare 自定义域名 (<code inline="">example.com</code>)</strong>，可以使用 <strong>Cloudflare Workers 代理请求</strong>，步骤如下：</p>
<hr>
<h2><strong>方法 1：使用 Cloudflare Workers 代理 ngrok</strong></h2>
<h3><strong>1. 进入 Cloudflare Workers</strong></h3>
<ol>
<li>登录 <a href="https://dash.cloudflare.com/">Cloudflare Dashboard</a>。</li>
<li>选择你的域名，例如 <code inline="">example.com</code>。</li>
<li>在左侧菜单找到 <strong>Workers &amp; Pages</strong>，点击 <strong>Create Application</strong>。</li>
</ol>
<hr>
<h3><strong>2. 创建一个 Workers 服务</strong></h3>
<ol>
<li>点击 <strong>"Create a Service"</strong>，选择 <strong>HTTP Handler</strong>，然后点击 <strong>"Create Service"</strong>。</li>
<li>在代码编辑器中，删除默认代码，并粘贴以下代码：</li>
</ol>
<pre><code class="language-javascript">export default {
  async fetch(request) {
    // 将 Cloudflare 请求代理到 ngrok 地址
    let url = new URL(request.url);
    url.hostname = "xxxxxx.ngrok-free.app";

    let modifiedRequest = new Request(url, request);
    return fetch(modifiedRequest);
  }
};
</code></pre>
<hr>
<h3><strong>3. 绑定 Workers 到自定义域名</strong></h3>
<ol>
<li>进入 <strong>Workers &amp; Pages</strong> → <strong>Routes</strong>。</li>
<li>添加一个新的路由，例如：
<pre><code>example.com/*
</code></pre>
</li>
<li>选择你刚刚创建的 Worker 服务，点击 <strong>Save</strong>。</li>
</ol>
<hr>
<h3><strong>4. 访问你的域名</strong></h3>
<p>现在，访问 <code inline="">https://example.com</code>，Cloudflare Workers 会自动代理 <code inline="">https://xxxxxx.ngrok-free.app</code>，你就可以通过自己的域名访问 ngrok 了！🚀</p>
<hr>
<h2><strong>方法 2：使用 Cloudflare Page Rules（仅 HTTP 转发）</strong></h2>
<p>如果你的 <strong>ngrok 是公网可访问的</strong>，你可以直接用 <strong>Cloudflare Page Rules</strong> 进行 <strong>301/302 重定向</strong>：</p>
<ol>
<li>进入 Cloudflare 控制台，选择你的域名 (<code inline="">example.com</code>)。</li>
<li>进入 <strong>Rules</strong> → <strong>Page Rules</strong>，点击 <strong>"Create Page Rule"</strong>。</li>
<li>规则设置：
<ul>
<li><strong>If the URL matches:</strong> <code inline="">https://example.com/*</code></li>
<li><strong>Then the settings are:</strong> <code inline="">Forwarding URL (302)</code> → <code inline="">https://xxxxxx.ngrok-free.app/$1</code></li>
</ul>
</li>
<li>点击 <strong>"Save and Deploy"</strong>。</li>
</ol>
<p>这种方式 <strong>不会隐藏 ngrok 地址</strong>，只是让 <code inline="">example.com</code> 自动跳转到 <code inline="">ngrok</code>，但 <strong>推荐使用方法 1（Workers 代理），这样可以保留自定义域名</strong>。</p>
<hr>
<h2><strong>总结</strong></h2>

方法 | 适用场景 | 优点 | 缺点
-- | -- | -- | --
Cloudflare Workers 代理（推荐） | 想要在 example.com 直接访问 ngrok | 域名不会跳转，支持 HTTPS | 可能有少量 Workers 请求费用
Cloudflare Page Rules 重定向 | 只是想跳转，不介意暴露 ngrok 地址 | 简单易用，免费 | 用户会看到 ngrok 地址


<p>如果你想用 <strong>Cloudflare 自定义域名完整代理 ngrok 地址</strong>，建议使用 <strong>Cloudflare Workers</strong> 方法！🚀</p></body></html><!--EndFragment-->
</body>
