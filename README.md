# Aether Avatar - Navigation Page

This repository maintains the main navigation page for Aether Avatar research projects.

## Repository Structure

```
├── index.html          # Main navigation page
├── projects.json       # Centralized project registry
└── README.md           # This file
```

## How It Works

### Centralized Project Registry (`projects.json`)

All projects are registered in `projects.json`. The navigation page dynamically loads projects from this file. When you add a new project, simply add a new entry to this JSON file.

### Adding a New Project

Edit `projects.json` and add a new entry:

```json
{
  "id": "your-project-id",
  "name": "Your Project Name",
  "url": "https://your-project-repo.github.io/",
  "desc": "Brief description of your project",
  "venue": "Conference/Journal Info",
  "authors": "Author List",
  "video": "path/to/video.mp4",
  "poster": "path/to/poster.png",
  "links": {
    "project": "https://your-project-repo.github.io/",
    "paper": "https://arxiv.org/...",
    "code": "https://github.com/..."
  }
}
```

### For Sub-Repository Maintainers

To add a "More Related Research" section to your project page, include the following JavaScript snippet in your HTML:

```html
<!-- Add this where you want the related research section to appear -->
<section id="related-works">
  <h2>More Related Research</h2>
  <div id="related-list">Loading...</div>
</section>

<script>
// Fetch projects.json from the main navigation page
fetch('https://aether-avatar.github.io/projects.json')
  .then(res => res.json())
  .then(projects => {
    // Filter out the current project based on URL
    const currentUrl = window.location.href;
    const others = projects.filter(p => !currentUrl.includes(p.id));
    
    const container = document.getElementById('related-list');
    if (others.length === 0) {
      container.innerHTML = '<p>No related projects found.</p>';
      return;
    }
    
    container.innerHTML = others.map(p => `
      <div class="related-project">
        <h3><a href="${p.url}">${p.name}</a></h3>
        <p>${p.desc}</p>
        <p><em>${p.authors}</em></p>
        <p>${p.venue}</p>
      </div>
    `).join('');
  })
  .catch(err => {
    console.error('Failed to load related projects:', err);
    document.getElementById('related-list').innerHTML = '<p>Failed to load related projects.</p>';
  });
</script>
```

### Important Notes

1. **CORS**: If your sub-repository is hosted on a different domain, ensure the main navigation page's server is configured to allow CORS requests for `projects.json`.

2. **URL Pattern**: The filtering uses the project `id` to exclude the current project. Make sure your project's `id` appears in your sub-repository's URL.

3. **Absolute URLs**: Use absolute URLs in `projects.json` for cross-repository compatibility.

## Deployment

This repository is designed to be deployed as a GitHub Pages site. The `projects.json` file should be accessible at the root of the deployed site (e.g., `https://aether-avatar.github.io/projects.json`).
