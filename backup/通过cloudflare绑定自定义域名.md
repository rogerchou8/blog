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
</html>要将 **ngrok 地址 (`https://xxxxxx.ngrok-free.app`)** 绑定到 **Cloudflare 自定义域名 (`example.com`)**，可以使用 **Cloudflare Workers 代理请求**，步骤如下：  

---

## **方法 1：使用 Cloudflare Workers 代理 ngrok**
### **1. 进入 Cloudflare Workers**
1. 登录 [[Cloudflare Dashboard](https://dash.cloudflare.com/)](https://dash.cloudflare.com/)。  
2. 选择你的域名，例如 `example.com`。  
3. 在左侧菜单找到 **Workers & Pages**，点击 **Create Application**。  

---

### **2. 创建一个 Workers 服务**
1. 点击 **"Create a Service"**，选择 **HTTP Handler**，然后点击 **"Create Service"**。  
2. 在代码编辑器中，删除默认代码，并粘贴以下代码：  

```javascript
export default {
  async fetch(request) {
    // 将 Cloudflare 请求代理到 ngrok 地址
    let url = new URL(request.url);
    url.hostname = "xxxxxx.ngrok-free.app";

    let modifiedRequest = new Request(url, request);
    return fetch(modifiedRequest);
  }
};
```

---

### **3. 绑定 Workers 到自定义域名**
1. 进入 **Workers & Pages** → **Routes**。  
2. 添加一个新的路由，例如：
   ```
   example.com/*
   ```
3. 选择你刚刚创建的 Worker 服务，点击 **Save**。  

---

### **4. 访问你的域名**
现在，访问 `https://example.com`，Cloudflare Workers 会自动代理 `https://xxxxxx.ngrok-free.app`，你就可以通过自己的域名访问 ngrok 了！🚀  

---

## **方法 2：使用 Cloudflare Page Rules（仅 HTTP 转发）**
如果你的 **ngrok 是公网可访问的**，你可以直接用 **Cloudflare Page Rules** 进行 **301/302 重定向**：
1. 进入 Cloudflare 控制台，选择你的域名 (`example.com`)。  
2. 进入 **Rules** → **Page Rules**，点击 **"Create Page Rule"**。  
3. 规则设置：
   - **If the URL matches:** `https://example.com/*`  
   - **Then the settings are:** `Forwarding URL (302)` → `https://xxxxxx.ngrok-free.app/$1`  
4. 点击 **"Save and Deploy"**。  

这种方式 **不会隐藏 ngrok 地址**，只是让 `example.com` 自动跳转到 `ngrok`，但 **推荐使用方法 1（Workers 代理），这样可以保留自定义域名**。

---

## **总结**
| 方法 | 适用场景 | 优点 | 缺点 |
|------|--------|------|------|
| **Cloudflare Workers 代理（推荐）** | 想要在 `example.com` 直接访问 ngrok | **域名不会跳转，支持 HTTPS** | 可能有少量 Workers 请求费用 |
| **Cloudflare Page Rules 重定向** | 只是想跳转，不介意暴露 ngrok 地址 | **简单易用，免费** | **用户会看到 ngrok 地址** |

如果你想用 **Cloudflare 自定义域名完整代理 ngrok 地址**，建议使用 **Cloudflare Workers** 方法！🚀