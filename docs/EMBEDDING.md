# Embedding Guide

The `index-embed.html` file is a streamlined version of the timeline designed specifically for embedding in other pages, documentation, or portfolios.

> 🎨 **[View Live Theme Demo](theme-demo.html)** - See all theme options in action!

## What's Different?

### Full Version (`index.html`)
- ✅ "Git History Timeline" header
- ✅ Theme toggle button (🌙/☀️)
- ✅ Footer with links
- ✅ Complete standalone page

### Embed Version (`index-embed.html`)
- ❌ No "Git History Timeline" header text
- ❌ No theme toggle button
- ❌ No footer
- ❌ No `min-height: 100vh` (wraps content height)
- ✅ Username display (`@username`)
- ✅ Statistics bar (commits, repos, years)
- ✅ Full contribution timeline
- ✅ Interactive tooltips
- ✅ Responsive design
- ✅ Self-contained (no external dependencies)
- ✅ **Theme configuration via URL parameter** (`?theme=light` or `?theme=dark`)
- ✅ **Auto-height via postMessage** (iframe resizes to content)

## Auto-Height Feature

The embed version automatically communicates its content height to the parent page via `postMessage`. This allows the iframe to resize dynamically without scrollbars.

### Setup Auto-Height

Add this JavaScript to your parent page to enable auto-height:

```html
<iframe 
  id="timeline-embed"
  src="https://yourusername.github.io/git-history-timeline/index-embed.html" 
  width="100%" 
  frameborder="0"
  style="border: 1px solid #30363d; border-radius: 6px;">
</iframe>

<script>
  // Listen for height updates from the embed
  window.addEventListener('message', function(event) {
    if (event.data.type === 'resize') {
      const iframe = document.getElementById('timeline-embed');
      iframe.style.height = event.data.height + 'px';
    }
  });
</script>
```

The embed will automatically:
- Send its height on initial load
- Update height when window is resized
- Adjust height when content changes
- Use `min-height: auto` instead of `100vh` for proper content wrapping

### Without Auto-Height

If you prefer a fixed height or can't use JavaScript:

```html
<iframe 
  src="https://yourusername.github.io/git-history-timeline/index-embed.html" 
  width="100%" 
  height="800" 
  frameborder="0"
  style="border: 1px solid #30363d; border-radius: 6px;">
</iframe>
```

The iframe will have scrollbars if the content exceeds 800px.

## Usage Examples

### 1. iframe Embedding

**Basic embed (fixed height):**

```html
<iframe 
  src="https://yourusername.github.io/git-history-timeline/index-embed.html" 
  width="100%" 
  height="800" 
  frameborder="0"
  style="border: 1px solid #30363d; border-radius: 6px;">
</iframe>
```

**With auto-height (recommended):**

```html
<iframe 
  id="git-timeline"
  src="https://yourusername.github.io/git-history-timeline/index-embed.html" 
  width="100%" 
  frameborder="0"
  style="border: 1px solid #30363d; border-radius: 6px;">
</iframe>

<script>
  window.addEventListener('message', function(event) {
    if (event.data.type === 'resize') {
      document.getElementById('git-timeline').style.height = event.data.height + 'px';
    }
  });
</script>
```

**With theme configuration and auto-height:**

```html
<!-- Light theme -->
<iframe 
  id="git-timeline-light"
  src="https://yourusername.github.io/git-history-timeline/index-embed.html?theme=light" 
  width="100%" 
  frameborder="0"
  style="border: 1px solid #d0d7de; border-radius: 6px;">
</iframe>

<!-- Dark theme -->
<iframe 
  id="git-timeline-dark"
  src="https://yourusername.github.io/git-history-timeline/index-embed.html?theme=dark" 
  width="100%" 
  frameborder="0"
  style="border: 1px solid #30363d; border-radius: 6px;">
</iframe>

<script>
  window.addEventListener('message', function(event) {
    if (event.data.type === 'resize') {
      // Handle both iframes
      const lightIframe = document.getElementById('git-timeline-light');
      const darkIframe = document.getElementById('git-timeline-dark');
      if (lightIframe) lightIframe.style.height = event.data.height + 'px';
      if (darkIframe) darkIframe.style.height = event.data.height + 'px';
    }
  });
</script>
```

### 2. Direct HTML Inclusion

Since the file is self-contained, you can include it directly:

```html
<!-- In your HTML page -->
<div id="git-timeline">
  <!-- Paste the contents of index-embed.html here -->
</div>
```

### 3. Markdown (GitHub README)

For GitHub README files:

```markdown
## My Contribution Timeline

<iframe 
  src="https://yourusername.github.io/git-history-timeline/index-embed.html" 
  width="100%" 
  height="800" 
  frameborder="0">
</iframe>
```

**Note:** GitHub README doesn't support iframes. Instead, you can:
- Link to the full page: `[View My Timeline](https://yourusername.github.io/git-history-timeline/)`
- Take a screenshot and embed it as an image
- Use GitHub's built-in contribution graph

### 4. Portfolio/Personal Website

```html
<!DOCTYPE html>
<html>
<head>
  <title>My Portfolio</title>
  <style>
    body {
      background: #ffffff;
      color: #1f2328;
    }
    .timeline-container {
      max-width: 900px;
      margin: 2rem auto;
      padding: 1rem;
    }
  </style>
</head>
<body>
  <section class="timeline-container">
    <h2>My GitHub Contributions</h2>
    <!-- Use ?theme=light to match light-themed portfolio -->
    <iframe 
      src="./index-embed.html?theme=light" 
      width="100%" 
      height="800" 
      frameborder="0"
      style="border: 1px solid #d0d7de; border-radius: 6px;">
    </iframe>
  </section>
</body>
</html>
```

### 5. Documentation Sites (MkDocs, Docusaurus, etc.)

Most documentation frameworks support HTML:

```markdown
# My Contributions

<!-- Match your docs theme -->
<iframe 
  src="https://yourusername.github.io/git-history-timeline/index-embed.html?theme=light" 
  width="100%" 
  height="800" 
  frameborder="0"
  style="border: 1px solid #d0d7de; border-radius: 6px;">
</iframe>
```

## Real-World Examples

### Light-Themed Blog

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    body {
      background: #ffffff;
      color: #24292f;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    }
    .content {
      max-width: 800px;
      margin: 0 auto;
      padding: 2rem;
    }
  </style>
</head>
<body>
  <div class="content">
    <h1>My Coding Journey</h1>
    <p>Here's my complete GitHub contribution history:</p>
    
    <!-- Light theme with auto-height -->
    <iframe 
      id="timeline"
      src="https://yourusername.github.io/git-history-timeline/index-embed.html?theme=light" 
      width="100%" 
      frameborder="0"
      style="border: 1px solid #d0d7de; border-radius: 6px; margin: 2rem 0;">
    </iframe>
  </div>
  
  <script>
    window.addEventListener('message', function(event) {
      if (event.data.type === 'resize') {
        document.getElementById('timeline').style.height = event.data.height + 'px';
      }
    });
  </script>
</body>
</html>
```

### Dark-Themed Portfolio

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    body {
      background: #0d1117;
      color: #f0f6fc;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    }
    .portfolio {
      max-width: 1200px;
      margin: 0 auto;
      padding: 2rem;
    }
  </style>
</head>
<body>
  <div class="portfolio">
    <h1>Developer Portfolio</h1>
    <p>My contribution timeline across all projects:</p>
    
    <!-- Dark theme with auto-height -->
    <iframe 
      id="timeline"
      src="https://yourusername.github.io/git-history-timeline/index-embed.html?theme=dark" 
      width="100%" 
      frameborder="0"
      style="border: 1px solid #30363d; border-radius: 6px; margin: 2rem 0;">
    </iframe>
  </div>
  
  <script>
    window.addEventListener('message', function(event) {
      if (event.data.type === 'resize') {
        document.getElementById('timeline').style.height = event.data.height + 'px';
      }
    });
  </script>
</body>
</html>
```

### Adaptive Theme (Match System Preference)

```html
<!-- No theme parameter - will adapt to user's system preference -->
<iframe 
  src="https://yourusername.github.io/git-history-timeline/index-embed.html" 
  width="100%" 
  height="800" 
  frameborder="0"
  style="border: 1px solid var(--border-color); border-radius: 6px;">
</iframe>
```

## Styling Tips

### Custom Container

Wrap the iframe in a styled container:

```html
<div style="
  background: #0d1117;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.3);
">
  <h2 style="color: #f0f6fc; margin-bottom: 1rem;">My Contributions</h2>
  <iframe 
    src="https://yourusername.github.io/git-history-timeline/index-embed.html" 
    width="100%" 
    height="800" 
    frameborder="0"
    style="border: 1px solid #30363d; border-radius: 6px;">
  </iframe>
</div>
```

### Responsive Height

Adjust height based on content:

```html
<iframe 
  src="https://yourusername.github.io/git-history-timeline/index-embed.html" 
  width="100%" 
  height="600" 
  frameborder="0"
  style="min-height: 600px; max-height: 1200px; border: 1px solid #30363d; border-radius: 6px;">
</iframe>
```

## Theme Configuration

The embed version supports theme configuration via URL parameters:

### Force Light Theme

```html
<iframe 
  src="https://yourusername.github.io/git-history-timeline/index-embed.html?theme=light" 
  width="100%" 
  height="800" 
  frameborder="0">
</iframe>
```

### Force Dark Theme

```html
<iframe 
  src="https://yourusername.github.io/git-history-timeline/index-embed.html?theme=dark" 
  width="100%" 
  height="800" 
  frameborder="0">
</iframe>
```

### Default Behavior (No Parameter)

Without the `?theme` parameter:
- Respects user's saved preference (if they visited before)
- Falls back to system theme preference
- Dark mode by default

### Theme Parameter Options

| Parameter | Result |
|-----------|--------|
| `?theme=light` | Forces light theme |
| `?theme=dark` | Forces dark theme |
| No parameter | Auto (saved preference → system preference → dark) |

This is especially useful when embedding in:
- **Light-themed sites** - Use `?theme=light` to match your site
- **Dark-themed sites** - Use `?theme=dark` to match your site
- **Documentation** - Force consistent theme across all embeds

## Performance

The embed file is:
- **Self-contained** - No external requests
- **Lightweight** - Pure HTML/CSS/JS, no frameworks
- **Fast loading** - Typically < 100KB for years of data
- **No tracking** - Zero analytics or external scripts

## Security

When embedding:
- The iframe is sandboxed by default (browser security)
- No cookies or localStorage access from parent page
- No form submissions or popups
- Read-only visualization

## Hosting Options

### GitHub Pages (Recommended)
```
https://yourusername.github.io/git-history-timeline/index-embed.html
```

### Custom Domain
```
https://timeline.yourdomain.com/index-embed.html
```

### CDN
Upload to any CDN (Cloudflare, Netlify, Vercel):
```
https://cdn.yourdomain.com/timeline/index-embed.html
```

### Local File
For local development or offline use:
```html
<iframe src="file:///path/to/index-embed.html"></iframe>
```

## Troubleshooting

### iframe shows blank page

1. Check the URL is correct and accessible
2. Verify CORS settings if hosting on different domain
3. Check browser console for errors

### Height is too small/large

Adjust the `height` attribute:
```html
<iframe height="1000" ...>  <!-- Increase for more years -->
```

### Styling doesn't match my site

The embed version uses GitHub's color scheme. To customize:
1. Download `index-embed.html`
2. Edit the CSS variables in the `<style>` section
3. Host the modified version

### Want to add theme toggle back

Use the full version (`index.html`) instead, or:
1. Copy the theme toggle code from `index.html`
2. Add it to your embedding page
3. Use `postMessage` to communicate with the iframe

## Examples in the Wild

After deploying, your embed URL will be:
```
https://<username>.github.io/git-history-timeline/index-embed.html
```

Share your timeline anywhere:
- Personal websites
- Portfolio pages
- Blog posts
- Documentation
- Presentations
- Social media (as a link)

## Advanced: Custom Styling

If you need to customize the embed version:

1. Generate the files locally
2. Edit `dist/index-embed.html`
3. Modify CSS variables in the `:root` section:

```css
:root {
  --bg-primary: #0d1117;      /* Main background */
  --bg-secondary: #161b22;    /* Card background */
  --text-primary: #f0f6fc;    /* Main text */
  --accent: #58a6ff;          /* Links and accents */
  /* ... more variables ... */
}
```

4. Host the modified version on your own server

## Questions?

For more information:
- Main README: [README.md](../README.md)
- GitHub Issues: [Report a problem](https://github.com/kibotu/git-history-timeline/issues)
