---
layout: galleries
title: Galleries
permalink: /galleries/
---

<section id="galleries-all">
  <h1>Galleries</h1>
  <div class="section-body">
    <ul class="gallery-list" id="gallery-list"></ul>
    <div id="pagination" style="display:flex; gap:8px; justify-content:center; margin-top:20px; flex-wrap:wrap;"></div>
  </div>
</section>

<script>
  const galleries = [
    {% assign sorted_contents = site.contents | sort: "title" | reverse %}
    {% for content in sorted_contents %}
    {
      url: "{{ content.url | relative_url }}",
      title: "{{ content.title | escape }}",
      day: "{{ content.day | escape }}"
    }{% unless forloop.last %},{% endunless %}
    {% endfor %}
  ];

  const PER_PAGE = 20;
  let currentPage = 1;

  function totalPages() {
    return Math.ceil(galleries.length / PER_PAGE);
  }

  function renderList(page) {
    const list = document.getElementById('gallery-list');
    const start = (page - 1) * PER_PAGE;
    const items = galleries.slice(start, start + PER_PAGE);
    list.innerHTML = items.map(g => `
      <li>
        <a href="${g.url}">${g.day}： ${g.title}</a>
      </li>
    `).join('');
  }

  function renderPagination(page) {
    const total = totalPages();
    const pag = document.getElementById('pagination');
    if (total <= 1) { pag.innerHTML = ''; return; }

    let html = '';
    html += `<button class="pag-btn" ${page === 1 ? 'disabled' : ''} onclick="goPage(${page - 1})">← 前へ</button>`;
    for (let i = 1; i <= total; i++) {
      html += `<button class="pag-btn ${i === page ? 'pag-active' : ''}" onclick="goPage(${i})">${i}</button>`;
    }
    html += `<button class="pag-btn" ${page === total ? 'disabled' : ''} onclick="goPage(${page + 1})">次へ →</button>`;
    pag.innerHTML = html;
  }

  function goPage(page) {
    currentPage = page;
    renderList(page);
    renderPagination(page);
    window.scrollTo({ top: 0, behavior: 'smooth' });
  }

  renderList(currentPage);
  renderPagination(currentPage);
</script>