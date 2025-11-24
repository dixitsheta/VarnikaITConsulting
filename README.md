# Varnika IT Consulting Website

Professional SAP consulting website built with Hugo static site generator, featuring golden branding and comprehensive service pages.

## 🌟 Project Overview

**Varnika means Pure Gold in Sanskrit** - A premium SAP Analytics Cloud, Datasphere, BW/4HANA, and custom widget development consulting website.

- **Tech Stack:** Hugo Extended v0.152.2 + Static HTML/CSS
- **Color Theme:** Navy (#003366) + Gold (#F4C430) + Light Gray (#F7F9FC)
- **Deployment:** GitHub Actions → FTP to Hostinger
- **CMS:** Decap CMS (browser-based, Git-backed)
- **Target Markets:** EU, Americas, Middle East

## 🚀 Quick Start

### Prerequisites

- Hugo Extended v0.120+ installed (`brew install hugo`)
- Git

### Local Development

```bash
# Clone the repository
git clone https://github.com/dixitsheta/VarnikaITConsulting.git
cd VarnikaITConsulting

# Start Hugo development server
hugo server -D

# Open browser to http://localhost:1313
```

### Building for Production

```bash
# Build static files to /public directory
hugo --minify

# Output will be in /public/ directory ready for deployment
```

## 📁 Project Structure

```
/
├── content/              # Markdown content files
│   ├── _index.md        # Homepage
│   ├── about.md         # About Us page
│   ├── contact.md       # Contact form page
│   ├── thank-you.md     # Form submission thank you
│   ├── services/        # Service pages
│   │   ├── sap-datasphere.md
│   │   ├── sac-analytics-cloud.md
│   │   ├── sac-custom-widgets.md
│   │   ├── bw4hana-modernization.md
│   │   └── afo-embedded-analytics.md
│   ├── blog/            # Blog posts (to be added)
│   └── case-studies/    # Case studies (to be added)
├── layouts/             # HTML templates
│   ├── _default/        # Default layouts
│   ├── partials/        # Reusable components (header, footer, SEO)
│   └── shortcodes/      # Custom shortcodes
├── static/              # Static assets
│   ├── css/             # Stylesheets
│   ├── images/logo/     # Logo files (upload yours here)
│   └── js/              # JavaScript (if needed)
├── assets/              # Hugo pipeline assets
│   └── scss/            # SCSS files
├── hugo.toml            # Hugo configuration
└── README.md            # This file
```

## 🎨 Logo Setup

Upload your logo files to `/static/images/logo/`:

- `logo-dark.svg` or `logo-dark.png` - For light backgrounds
- `logo-light.svg` or `logo-light.png` - For dark backgrounds

Recommended: SVG format (scalable) or PNG at 400-800px width for retina displays.

## 📝 Content Editing

### Direct Editing (Developers)

Edit `.md` files in `/content/` directory:

```bash
# Edit homepage
nano content/_index.md

# Edit service page
nano content/services/sap-datasphere.md

# Preview changes
hugo server -D
```

### Via Decap CMS (Non-Technical Users)

1. Navigate to `https://varnikaitconsulting.com/admin/`
2. Login with GitHub OAuth
3. Edit content through visual interface
4. Changes auto-commit to Git and trigger deployment

## 🌐 Deployment

### Automated Deployment (GitHub Actions)

Configured in `.github/workflows/deploy.yml` (to be created in Phase 3):

1. Push to `main` branch
2. GitHub Actions builds Hugo site
3. Deploys to Hostinger via FTP automatically

### Manual Deployment

```bash
# Build the site
hugo --minify

# Upload /public/ contents to Hostinger via FTP
# Server: ftp://145.223.17.184
# Directory: /public_html/
```

## 🛠️ Configuration

Key settings in `hugo.toml`:

- **baseURL:** `https://varnikaitconsulting.com/`
- **Contact Email:** sales@varnikaitconsulting.com
- **Color Theme:** Defined in `/static/css/site.css`
- **Navigation:** Configured in `[menu.main]` section

## 🎨 Color Palette (Varnika Golden Theme)

```css
--color-navy: #003366;      /* Primary: Headings, nav */
--color-gold: #F4C430;       /* Accent: CTAs, highlights */
--color-gold-dark: #DAA520;  /* Hover states */
--color-bg-light: #F7F9FC;   /* Section backgrounds */
--color-text-dark: #1A1A1A;  /* Body text */
--color-text-gray: #666666;  /* Secondary text */
--color-white: #FFFFFF;      /* Backgrounds */
```

## 📊 Site Pages

### ✅ Completed (Phase 1)
- [x] Homepage with hero, services grid, testimonials
- [x] About Us
- [x] Contact (FormSubmit integration)
- [x] Thank You (post-form submission)
- [x] Services landing page
- [x] SAP Datasphere service page
- [x] SAC Analytics Cloud service page
- [x] SAC Custom Widgets service page
- [x] BW/4HANA Modernization service page
- [x] Embedded Analytics (AFO) service page

### 🔄 In Progress (Phase 2)
- [ ] Blog infrastructure
- [ ] Case Studies section
- [ ] Decap CMS configuration
- [ ] Search functionality (Pagefind)

### 📅 Planned (Phase 3)
- [ ] GitHub Actions CI/CD
- [ ] Google Analytics 4
- [ ] Privacy Policy & Terms
- [ ] FAQ page
- [ ] Resources/Downloads section

## 🔧 Development Commands

```bash
# Start dev server with drafts
hugo server -D

# Build for production with minification
hugo --minify

# Clean build cache
hugo --gc

# Create new blog post
hugo new blog/my-post-title.md

# Create new service page
hugo new services/my-service.md
```

## 📞 Support & Questions

**Developer:** Dixit Sheta  
**Email:** sales@varnikaitconsulting.com  
**Repository:** https://github.com/dixitsheta/VarnikaITConsulting  

## 📄 License

© 2025 Varnika IT Consulting. All rights reserved.

---

## Phase 1 Completion Summary ✅

**Week 1 - COMPLETED:**
- ✅ Git repository initialized and pushed to GitHub
- ✅ Hugo Extended installed and configured
- ✅ Golden color theme implemented (Navy + Gold)
- ✅ Homepage with hero, services, testimonials
- ✅ 5 comprehensive service pages
- ✅ About Us, Contact, Thank You pages
- ✅ Responsive header/footer navigation
- ✅ SEO meta tags (Open Graph, Twitter Cards)
- ✅ FormSubmit contact form integration
- ✅ Site running locally at http://localhost:1313

**Next Steps (Week 2-3):**
- Configure Decap CMS for content management
- Set up blog infrastructure
- Create 5-8 initial blog posts
- Integrate Pagefind search
- Prepare for GitHub Actions deployment

---

**Built with ❤️ using Hugo Static Site Generator**
