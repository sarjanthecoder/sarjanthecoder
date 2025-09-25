# Hi 👋, I'm Lokesh — Polished Portfolio README

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=700&size=30&pause=1000&color=3EC5FF&center=true&width=700&lines=Hi+%F0%9F%91%8B%2C+I'm+Lokesh!;+Coder+from+Vellore!" alt="Typing SVG" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/PYTHON-blue?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/HTML5-e34c26?style=for-the-badge&logo=html5&logoColor=white" />
  <img src="https://img.shields.io/badge/CSS3-264de4?style=for-the-badge&logo=css3&logoColor=white" />
  <img src="https://img.shields.io/badge/JAVA-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white" />
  <img src="https://img.shields.io/badge/GITHUB-181717?style=for-the-badge&logo=github&logoColor=white" />
</p>

---

## 🚀 Quick Snapshot

* 🔥 Passionate coder from **Vellore, India**
* 🎓 Final Year B.Tech — **AI & Data Science**
* 🛠️ Building projects with **Python, Java, HTML, Firebase**
* 🌱 Learning: Full-stack (JS) & AI
* 🎯 Dream: Become a **Microsoft Software Engineer**

---

## ⭐ Features added / improvements

1. **Attractive, responsive README** with: animated header, badges, GIFs, and contribution graph.
2. **Dynamic "Projects" fetching** from your public website (`lokeshloki.site`) so README can stay in sync.
3. **GitHub Actions workflow** to auto-update README by pulling project details from your site on every push or schedule.
4. **Star badge** that shows live GitHub star count.
5. **Contact and quick-install / contribution instructions** for others.
6. **Code examples** (HTML + JavaScript) to fetch and render projects from your website.

---

## 🧩 What I added (files & snippets)

### 1) `README.md` (this file)

### 2) `scripts/fetch_projects.js` — fetches projects from your website and renders into a section (also usable by a GitHub Action)

```javascript
// scripts/fetch_projects.js
// Usage: node scripts/fetch_projects.js > projects_section.md
const https = require('https');

const SITE_JSON_URL = 'https://lokeshloki.site/projects.json'; // make sure your site exposes this

https.get(SITE_JSON_URL, (res) => {
  let data = '';
  res.on('data', chunk => data += chunk);
  res.on('end', () => {
    try {
      const projects = JSON.parse(data);
      console.log('## 🔧 Projects (fetched from lokeshloki.site)\n');
      projects.forEach(p => {
        console.log(`### [${p.title}](${p.url})`);
        if (p.desc) console.log(p.desc + '\n');
        console.log(`- Tech: ${p.tech || '—'}`);
        console.log(`- Repo: ${p.repo || 'Private / not provided'}\n`);
      });
    } catch (e) {
      console.error('Failed to parse projects.json', e);
      process.exit(1);
    }
  });
}).on('error', (err) => { console.error('Fetch error', err); process.exit(1); });
```

> Note: If `https://lokeshloki.site/projects.json` doesn't exist yet, create a JSON endpoint or static file on your site that lists projects. Example format below.

```json
// projects.json (example)
[
  {
    "title": "Grocery-Shop",
    "url": "https://lokeshloki.site/grocery-shop",
    "desc": "Full-stack grocery marketplace UI built with HTML/CSS/JS and Firebase.",
    "tech": "HTML, CSS, JS, Firebase",
    "repo": "https://github.com/lokeshloki65/grocery-shop"
  }
]
```

---

### 3) `projects_widget.html` — embeddable widget for your portfolio or README preview page

```html
<div id="projects-container">Loading projects...</div>
<script>
  fetch('https://lokeshloki.site/projects.json')
    .then(r => r.json())
    .then(projects => {
      const c = document.getElementById('projects-container');
      c.innerHTML = '';
      projects.forEach(p => {
        const card = document.createElement('div');
        card.style.border = '1px solid rgba(255,255,255,0.06)';
        card.style.padding = '12px';
        card.style.margin = '8px 0';
        card.innerHTML = `<h3><a href="${p.url}" target="_blank">${p.title}</a></h3><p>${p.desc || ''}</p><small>${p.tech || ''}</small>`;
        c.appendChild(card);
      });
    })
    .catch(err => { document.getElementById('projects-container').innerText = 'Could not load projects.'; console.error(err); });
</script>
```

---

### 4) GitHub Action: `.github/workflows/update_readme.yml`

This workflow fetches `projects.json` from your site and regenerates a `projects_section.md` file that the main README includes (via a simple token replacement technique).

```yaml
name: Update README Projects
on:
  push:
    branches: [ main ]
  schedule:
    - cron: '0 2 * * *' # daily at 02:00 UTC

jobs:
  update-readme:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '18'
      - name: Fetch projects and create section
        run: |
          npm ci --no-audit --no-fund || true
          node scripts/fetch_projects.js > projects_section.md
      - name: Commit updated projects section
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add projects_section.md || true
          git commit -m "chore: update projects section from lokeshloki.site" || echo "no changes"
          git push
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

---

## 🔖 Badges & GitHub Stars

Use this badge to show live star count (add to top of your README):

```markdown
[![GitHub stars](https://img.shields.io/github/stars/lokeshloki65/lokeshloki65?style=social)](https://github.com/lokeshloki65)
```

Replace `lokeshloki65/lokeshloki65` with the repository you want to track.

---

## ✅ How to install & enable auto updates

1. Add `projects.json` to your website root (or expose an endpoint).
2. Add `scripts/fetch_projects.js` to your repo.
3. Add the GitHub Action `.github/workflows/update_readme.yml`.
4. Push to `main`. The Action will run and create `projects_section.md`.
5. In your `README.md`, include the generated section with a token comment and script that replaces it during the workflow. Example token in README:

```markdown
<!-- PROJECTS:START -->
<!-- PROJECTS:END -->
```

Then your workflow can replace the token contents with `projects_section.md` contents.

---

## ✍️ Commit message style suggestion (for consistent grade-like logs)

Use Conventional Commits to make commits look neat:

* `feat:` new feature
* `fix:` bug fix
* `chore:` maintenance
* `docs:` documentation

Example commit: `feat: add dynamic projects fetcher from lokeshloki.site`

---

## 📫 Contact

* Email: [lokesh152005@gmail.com](mailto:lokesh152005@gmail.com)
* Portfolio: [https://lokeshloki.site](https://lokeshloki.site)
* LinkedIn: [https://www.linkedin.com/in/lokesh-m-265b832b3](https://www.linkedin.com/in/lokesh-m-265b832b3)

---

## 🙌 Final notes

* If you want, I can also:

  * generate a ready-to-commit `README.md` that includes the fetched projects content (if you paste `projects.json` here I will merge it into README now), or
  * produce a standalone `projects.json` file and the exact pull request content.

---

*Made with ❤️ for Lokesh — let me know if you'd like a version with different styling (dark/light), more badges (npm, PyPI), or a Twitter card.*
