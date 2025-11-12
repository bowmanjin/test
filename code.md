```html
{{ if and .Site.Params.lazyImage (eq .Section "posts") -}}
<script src="{{ .Site.Params.staticPrefix }}{{ "js/lazyload.min.js" | relURL }}"></script>
<script>
  document.addEventListener('DOMContentLoaded', function() {
  try {
      var root = document.getElementById('article') || document.querySelector('article') || document.body;
      if (typeof LazyLoad === 'function' && root) {
        new LazyLoad({ container: root });
        console.log('[lazyload] 初始化完成，容器 =', root.tagName || 'body');
      } else {
        console.warn('[lazyload] 跳过初始化：LazyLoad 未定义或容器不存在');
      }
    } catch(e) {
      console.warn('LazyLoad init skipped:', e);
    }
  });
</script>
<script>
  // Portal-style dropdown toggle for mobile and debug logs
  document.addEventListener('DOMContentLoaded', function() {
    const nav = document.querySelector('.mega-nav');
    if (!nav) return;
    console.log('[导航调试] mega-nav 初始化');
    const items = nav.querySelectorAll('.nav-item.has-children > .nav-link');
    items.forEach(link => {
      link.addEventListener('click', function(e) {
        // Toggle only on small screens
        if (window.matchMedia('(max-width: 900px)').matches) {
          e.preventDefault();
          const dd = this.parentElement.querySelector('.dropdown');
          const opened = dd && dd.style.display === 'block';
          // close all others
          nav.querySelectorAll('.dropdown').forEach(el => el.style.display = 'none');
          if (dd) dd.style.display = opened ? 'none' : 'block';
          console.log('[导航调试] 切换子菜单: ', this.textContent.trim());
        }
      }, false);
    });
    // 简单的点击外部关闭
    document.addEventListener('click', function(e){
      if (!nav.contains(e.target) && window.matchMedia('(max-width: 900px)').matches) {
        nav.querySelectorAll('.dropdown').forEach(el => el.style.display = 'none');
      }
    });
    // 在需要时可启用断点
    // debugger;
  });
</script>
{{ end }}
<script>window.__EXCERPT_QTY__ = {{ default 80 .Site.Params.excerpt_quantity }};</script>
<script src="{{ .Site.Params.staticPrefix }}{{ "js/excerpt-truncate.js" | relURL }}" defer></script>
<script>
  // 轻量级的同源链接预取：鼠标悬停或触屏时提前获取文档
  (function(){
    const isSameOrigin = (url) => {
      try { const u = new URL(url, location.href); return u.origin === location.origin; } catch(e){ return false; }
    };
    const ensurePrefetch = (href) => {
      if (!isSameOrigin(href)) return;
      if (document.querySelector(`link[rel="prefetch"][href="${href}"]`)) return;
      const l = document.createElement('link');
      l.rel = 'prefetch';
      l.href = href;
      l.as = 'document';
      document.head.appendChild(l);
    };
    document.addEventListener('mouseover', function(e){
      const a = e.target.closest && e.target.closest('a[href]');
      if (a) ensurePrefetch(a.href);
    }, { passive: true });
    document.addEventListener('touchstart', function(e){
      const a = e.target.closest && e.target.closest('a[href]');
      if (a) ensurePrefetch(a.href);
    }, { passive: true });
})();
</script>
<script>
  // Mobile overlay menu open/close + search
  document.addEventListener('DOMContentLoaded', function () {
    try {
      const toggle = document.getElementById('menuToggle');
      const overlay = document.getElementById('mobileMenu');
      const closeBtn = document.getElementById('mobileMenuClose');
      const searchInput = document.getElementById('mobileMenuSearch');
      const langToggle = document.getElementById('mobileLangToggle');
      const langDropdown = document.getElementById('mobileLangDropdown');
      const html = document.documentElement;
      const open = () => { overlay.classList.add('open'); html.classList.add('no-scroll'); toggle?.setAttribute('aria-expanded','true'); };
      const close = () => { overlay.classList.remove('open'); html.classList.remove('no-scroll'); toggle?.setAttribute('aria-expanded','false'); hideLangDropdown(); };
      const hideLangDropdown = () => { if (langDropdown) { langDropdown.hidden = true; langToggle?.setAttribute('aria-expanded','false'); } };
      const toggleLangDropdown = () => { if (!langDropdown) return; const isHidden = langDropdown.hidden; langDropdown.hidden = !isHidden; langToggle?.setAttribute('aria-expanded', String(!isHidden)); };
      toggle && toggle.addEventListener('click', function(e){ e.preventDefault(); overlay.classList.contains('open') ? close() : open(); });
      closeBtn && closeBtn.addEventListener('click', function(){ close(); });
      overlay && overlay.addEventListener('click', function(e){
        if (e.target === overlay) { close(); return; }
        if (!e.target.closest('.mobile-menu-extras')) { hideLangDropdown(); }
      });
      document.addEventListener('keydown', function(e){ if (e.key === 'Escape') { close(); hideLangDropdown(); } });
      if (langToggle) { langToggle.addEventListener('click', function(e){ e.preventDefault(); toggleLangDropdown(); }); }
      // 当视口恢复到桌面宽度时，自动关闭覆盖菜单，恢复桌面导航
      const ensureClosedOnDesktop = () => { if (window.innerWidth > 900) { close(); hideLangDropdown(); } };
      const mq = window.matchMedia('(max-width: 900px)');
      if (typeof mq.addEventListener === 'function') {
        mq.addEventListener('change', function(e){ if (!e.matches) close(); });
      } else if (typeof mq.addListener === 'function') {
        mq.addListener(function(e){ if (!e.matches) close(); });
      }
      window.addEventListener('resize', ensureClosedOnDesktop);
      // 搜索回车跳转到 /{lang}/search/?q=keyword
      if (searchInput) {
        searchInput.addEventListener('keydown', function(e){
          if (e.key === 'Enter') {
            const langPrefix = document.documentElement.lang?.startsWith('zh') ? '/cn' : '/en';
            const q = encodeURIComponent(searchInput.value.trim());
            window.location.href = `${langPrefix}/search/?q=${q}`;
          }
        });
      }

      // 缺失翻译提示与2秒跳转（桌面与移动共用）
      const missingLinks = document.querySelectorAll('a.dropdown-link[data-missing="true"]');
      function showMissingMessage(text){
        let box = document.getElementById('lang-missing-msg');
        if (!box) {
          box = document.createElement('div');
          box.id = 'lang-missing-msg';
          box.setAttribute('role', 'alert');
          box.style.position = 'fixed';
          box.style.top = '12px';
          box.style.left = '50%';
          box.style.transform = 'translateX(-50%)';
          box.style.background = '#222';
          box.style.color = '#fff';
          box.style.padding = '8px 12px';
          box.style.borderRadius = '6px';
          box.style.boxShadow = '0 2px 8px rgba(0,0,0,.2)';
          box.style.zIndex = '9999';
          document.body.appendChild(box);
        }
        box.textContent = text;
        box.style.display = 'block';
        console.log('[lang] 缺失翻译提示：', text);
      }
      missingLinks.forEach(function(a){
        a.addEventListener('click', function(e){
          e.preventDefault();
          const targetHref = a.getAttribute('href');
          const targetLang = a.getAttribute('data-target-lang') || (targetHref.includes('/en/') ? 'en' : 'cn');
          const uiLang = (document.documentElement.lang || '').toLowerCase().startsWith('zh') ? 'cn' : 'en';
          const text = (uiLang === 'cn' && targetLang === 'en')
            ? '该文章暂无英文版本，将在 2 秒后返回英文首页…'
            : 'No Chinese version for this article. Redirecting home in 2 seconds…';
          showMissingMessage(text);
          setTimeout(function(){ window.location.href = targetHref; }, 2000);
        });
      });
    } catch (err) {
      console.warn('[mobile menu] init failed:', err);
    }
  });
</script>
<script>
  // remove disqus ads
  document.addEventListener("DOMContentLoaded", function() {
    const disqus = document.getElementById('disqus_thread');
    if (!disqus) {
      return;
    }
    const observer = new MutationObserver(function(mutations) {
      mutations.forEach(function(mutation) {
        const iframes = disqus.getElementsByTagName('iframe');
        if (iframes.length > 1) {
          const commentsIframe = iframes[1];
          while (disqus.firstChild) {
            disqus.removeChild(disqus.firstChild);
          }
          disqus.appendChild(commentsIframe);
          observer.disconnect();
        }
      });
    });
    observer.observe(disqus, { childList: true, subtree: true });
  });
</script>

{{ if (and .IsPage (eq .Section "posts")) }}
<script>
  // 自动为正文标题添加层级数字，并构建左侧导航（默认显示，可隐藏/显示）
  document.addEventListener('DOMContentLoaded', function(){
    try {
      const content = document.querySelector('.post-content');
      if (!content) return;

      const headings = content.querySelectorAll('h1, h2, h3, h4, h5, h6');
      if (headings.length === 0) return;

      const counters = [0,0,0,0,0,0]; // h1..h6
      const toc = document.createElement('nav');
      toc.id = 'post-toc-left';
      toc.className = 'post-toc-left';
      toc.innerHTML = '<div class="toc-head"><strong>目录</strong><button id="toc-toggle" class="toc-toggle">隐藏</button></div><div class="toc-body"></div>';
      const tocBody = toc.querySelector('.toc-body');
      const rootUL = document.createElement('ul');
      rootUL.className = 'toc-list';
      tocBody.appendChild(rootUL);

      // 构建层级树并在父项上添加折叠图标
      const lastLiByLevel = {}; // 记录各级最后一个 li

      headings.forEach((h, idx) => {
        const level = parseInt(h.tagName.substring(1), 10);
        // reset lower levels
        for (let i = level; i < 6; i++) counters[i] = 0;
        // increment current level
        counters[level-1]++;
        // build number string up to current level
        const nums = counters.slice(0, level).filter(n => n>0).join('.');
        const text = h.textContent.trim();

        // ensure unique heading id
        if (!h.id) {
          const base = text.replace(/\s+/g,'-').replace(/[^\w\-\u4e00-\u9fa5]/g,'').toLowerCase();
          let candidate = base || `h-${nums.replace(/\./g,'-')}-${idx}`;
          // 如果已存在同名 id，则追加索引确保唯一
          if (document.getElementById(candidate)) {
            candidate = `${candidate}-${idx}`;
          }
          h.id = candidate;
        }

        // prepend number span
        const numSpan = document.createElement('span');
        numSpan.className = 'hn-num';
        // H1 标题的序号后加一个点，其它级别保持原样
        numSpan.textContent = level === 1 ? (nums + '.') : nums;
        h.insertBefore(numSpan, h.firstChild);

        // 选择父级 UL（若为子级，挂到上一层的最后一个 li 的 children 下）
        let parentUL = rootUL;
        if (level > 1) {
          const pLi = lastLiByLevel[level-1];
          if (pLi) {
            let childUL = pLi.querySelector(':scope > ul.toc-children');
            if (!childUL) {
              childUL = document.createElement('ul');
              childUL.className = 'toc-children';
              pLi.appendChild(childUL);
              pLi.classList.add('has-children', 'expanded');
              // 添加折叠/展开图标
              const line = pLi.querySelector(':scope > .toc-line');
              if (line && !line.querySelector('.toc-caret')) {
                const caret = document.createElement('span');
                caret.className = 'toc-caret';
                caret.textContent = '▾';
                line.insertBefore(caret, line.firstChild);
                caret.addEventListener('click', function(e){
                  e.preventDefault();
                  pLi.classList.toggle('collapsed');
                  pLi.classList.toggle('expanded');
                  caret.textContent = pLi.classList.contains('collapsed') ? '▸' : '▾';
                });
              }
            }
            parentUL = childUL;
          }
        }

        // 创建当前项 li + 行
        const li = document.createElement('li');
        li.className = `toc-li l${level}`;
        const line = document.createElement('div');
        line.className = 'toc-line';
        const a = document.createElement('a');
        a.href = `#${h.id}`;
        a.className = 'toc-link';
        // 左侧目录中的 H1 序号也加一个点，其它级别保持原样
        const displayNums = level === 1 ? (nums + '.') : nums;
        a.innerHTML = `<span class="toc-num">${displayNums}</span>${text}`;
        line.appendChild(a);
        li.appendChild(line);
        parentUL.appendChild(li);

        // 记录此层的最后一个 li，并清理更深层记录
        lastLiByLevel[level] = li;
        for (let k = level+1; k <= 6; k++) { delete lastLiByLevel[k]; }
      });

      // attach TOC
      document.body.appendChild(toc);

      // 让目录水平贴近正文左侧：根据正文容器的可视位置动态计算，避免与正文重叠
      try {
        const minLeft = 8;  // 稍微更靠左，释放更多可用宽度
        const gap = 8;      // 与正文更近一点，但不重叠
        const maxWidth = 320; // 允许更宽
        const minUsable = 80; // 小于此宽度则隐藏，避免入侵正文

        const place = () => {
          const rect = content.getBoundingClientRect();
          const avail = Math.round(rect.left - gap - minLeft); // 目录可用最大宽度
          if (avail <= 0) {
            toc.style.display = 'none';
            console.log('[TOC] 无可用空间，目录隐藏以避免入侵正文', { contentLeft: rect.left, avail });
            return;
          }
          toc.style.display = '';
          const desiredWidth = Math.round(Math.min(maxWidth, Math.max(minUsable, avail)));
          const left = Math.round(rect.left - gap - desiredWidth); // 保证右侧与正文留 gap
          toc.style.setProperty('width', desiredWidth + 'px', 'important');
          toc.style.setProperty('left', Math.max(minLeft, left) + 'px', 'important');
          toc.style.top = `${130}px`;
          console.log('[TOC] 定位并避免重叠：', { left, desiredWidth, contentLeft: rect.left, avail });
        };

        // 初始定位 + 响应式更新
        requestAnimationFrame(place);
        window.addEventListener('resize', place);
      } catch(e) {
        console.warn('[TOC] 动态定位失败，使用默认位置:', e);
      }

      // 样式回退：若未成功加载 .post-toc-left 的 CSS（position 非 fixed），则应用内联样式且同样进行不重叠定位
      try {
        const cs = window.getComputedStyle(toc);
        if (cs.position !== 'fixed') {
          toc.style.position = 'fixed';
          toc.style.top = '130px';
          toc.style.maxHeight = '70vh';
          // 长标题换行并隐藏横向滚动
          toc.style.overflowY = 'auto';
          toc.style.overflowX = 'hidden';
          toc.style.background = '#fff';
          toc.style.border = 'none';
          toc.style.borderRadius = '8px';
          toc.style.boxShadow = 'none';
          toc.style.padding = '8px 10px';
          toc.style.zIndex = '999';
          // 同步进行定位，避免与正文重叠
          const rect = content.getBoundingClientRect();
          const minLeft = 8, gap = 8, maxWidth = 320, minUsable = 80;
          const avail = Math.round(rect.left - gap - minLeft);
          if (avail <= 0) { toc.style.display = 'none'; }
          else {
            toc.style.display = '';
            const desiredWidth = Math.round(Math.min(maxWidth, Math.max(minUsable, avail)));
            const left = Math.round(rect.left - gap - desiredWidth);
            toc.style.left = Math.max(minLeft, left) + 'px';
            toc.style.width = desiredWidth + 'px';
          }
          console.log('[TOC] 样式回退已启用并完成定位：未检测到 CSS .post-toc-left');
        }
      } catch (e) {
        console.warn('[TOC] 样式回退检测异常:', e);
      }

      // toggle hide/show
      const tgl = document.getElementById('toc-toggle');
      tgl.addEventListener('click', function(){
        const hidden = toc.classList.toggle('collapsed');
        if (hidden) { this.textContent = '显示'; }
        else { this.textContent = '隐藏'; }
        console.log('[TOC] 切换状态：', hidden ? 'collapsed' : 'expanded');
      });

      // 当前可视标题加粗高亮：根据滚动动态更新 + 点击即刻高亮
      try {
        const liById = {};
        toc.querySelectorAll('.toc-link').forEach(a => {
          const id = a.getAttribute('href').slice(1);
          const li = a.closest('.toc-li');
          if (id && li) liById[id] = li;
        });
        let currentId = null;
        const setCurrent = (id) => {
          if (!id || currentId === id) return;
          // 移除所有已有 current，避免多个加粗
          toc.querySelectorAll('.toc-li.current').forEach(el => el.classList.remove('current'));
          const li = liById[id];
          if (li) li.classList.add('current');
          currentId = id;
        };
        const updateCurrent = () => {
          const viewportTop = 140; // 稍加缓冲
          let bestTop = -Infinity, bestId = null;
          headings.forEach(h => {
            const t = h.getBoundingClientRect().top;
            if (t <= viewportTop && t > bestTop) { bestTop = t; bestId = h.id; }
          });
          if (!bestId && headings.length) { bestId = headings[0].id; }
          setCurrent(bestId);
        };
        let raf = null;
        window.addEventListener('scroll', function(){
          if (raf) return;
          raf = requestAnimationFrame(function(){ raf = null; updateCurrent(); });
        }, { passive: true });
        // 点击目录项后立即设置 current，并在 hash 变化时兜底
        toc.addEventListener('click', function(e){
          const a = e.target.closest && e.target.closest('a.toc-link[href^="#"]');
          if (!a) return;
          const id = a.getAttribute('href').slice(1);
          setCurrent(id);
        });
        window.addEventListener('hashchange', function(){
          const id = location.hash ? decodeURIComponent(location.hash.slice(1)) : null;
          if (id) setCurrent(id);
        });
        updateCurrent();
        console.log('调试信息: 左侧目录与标题编号构建完成', { headings: headings.length });
      } catch(err){
        console.warn('[TOC] 当前标题高亮失败：', err);
      }
    } catch (err) {
      console.warn('调试信息: 构建目录失败', err);
      debugger; // 断点便于在开发者工具中查看状态
    }
  });
</script>
{{ end }}
```
