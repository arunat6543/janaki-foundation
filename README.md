# Janakiyamma Foundation Website

A professional website for the Janakiyamma Foundation, dedicated to empowering kids and adults to learn AI/ML. Built with modern web technologies and hosted on GitHub Pages.

## 🌐 Live Website

- **Production**: [https://janaki-foundation.org](https://janaki-foundation.org)
- **GitHub Pages**: [https://arunat6543.github.io/janaki-foundation](https://arunat6543.github.io/janaki-foundation)
- **Repository**: [https://github.com/arunat6543/janaki-foundation](https://github.com/arunat6543/janaki-foundation)

## 📁 Project Structure

```
janaki-foundation/
├── index.html          # Homepage - Mission statement and overview
├── about.html          # About page - Tribute to Janakiyamma
├── register.html       # Registration page - Google Form integration
├── portal.html         # Learning portal - Firebase authentication
├── styles.css          # All styling and responsive design
├── script.js           # Interactive functionality
├── images/             # Image assets folder
│   ├── README.md      # Image guidelines
│   ├── hero-background.jpg     # Homepage banner (add this)
│   ├── janakiyamma.jpg        # Grandmother's photo (add this)
│   └── logo.png               # Foundation logo (optional)
├── CNAME              # Custom domain configuration
└── README.md          # This file
```

## 🎯 Features

- ✅ **4 Complete Pages**: Home, About, Register, Portal
- ✅ **Responsive Design**: Works on all devices (mobile, tablet, desktop)
- ✅ **Custom Domain**: janaki-foundation.org
- ✅ **Firebase Authentication**: Google sign-in for portal access
- ✅ **Google Form Integration**: Ready for registration
- ✅ **JupyterHub Integration**: Placeholder for learning environment
- ✅ **Professional UI/UX**: Modern, accessible, fast-loading

## 🚀 Quick Start Guide

### Prerequisites
- Text editor (Notepad++, Visual Studio Code, or even Notepad)
- Git installed on your computer
- GitHub account (already set up)

### Making Your First Edit

1. **Open any HTML file** in a text editor
2. **Make your changes** (see editing guide below)
3. **Save the file**
4. **Push changes to live website**:
   ```bash
   git add .
   git commit -m "Description of your changes"
   git push
   ```
5. **Your changes go live in 2-3 minutes!**

## ✏️ Editing Guide

### 🏠 Homepage (index.html)

#### Change Main Title
**Line 47**: `<h1 class="hero-title">Empowering kids and adults to learn AI/ML</h1>`

#### Change Subtitle
**Line 48**: `<p class="hero-subtitle">Building the future of artificial intelligence education, one learner at a time</p>`

#### Change Mission Cards
**Lines 61-81**: Update the three mission cards:
- AI/ML Education
- Community Learning  
- Global Impact

#### Change Statistics
**Lines 90-105**: Update the stats section:
- Students Supported
- Free Learning
- Learning Access
- Potential

### 📖 About Page (about.html)

#### Change Page Title
**Line 43**: `<h1 class="page-title">About Our Foundation</h1>`

#### Update Tribute Text
**Lines 55-64**: Edit the tribute to Janakiyamma

#### Modify Story Content
**Lines 78-100**: Update "Our Story" section

### 📋 Register Page (register.html)

#### Add Google Form
1. Create form at [forms.google.com](https://forms.google.com)
2. Include fields: Name, Age, Email, Parent's Email
3. Get embed code from Google Forms
4. Replace placeholder in **lines 134-139**

### 🔐 Portal Page (portal.html)

#### Setup Firebase Authentication
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create new project
3. Enable Authentication > Google sign-in
4. Get configuration from Project Settings
5. Replace config in **lines 88-95**

#### Update JupyterHub Link
**Line 115**: Replace placeholder URL with your JupyterHub server

### 🎨 Styling (styles.css)

#### Change Colors
- **Primary Blue**: `#2563eb` (search and replace)
- **Secondary Blue**: `#3b82f6`
- **Green**: `#10b981`
- **Red**: `#ef4444`

#### Change Fonts
**Line 8**: Replace `'Inter'` with your preferred Google Font

### 📧 Contact Information

#### Change Email Address
**Line 129 (all pages)**: `info@janaki-foundation.org`

#### Change Footer Text
**Line 134 (all pages)**: Update copyright and description

## 📸 Adding Images

### Required Images
1. **Homepage Background**: `images/hero-background.jpg`
   - Size: 1920x1080 pixels (landscape)
   - Subject: Students learning, education, or AI/ML themed

2. **Janakiyamma's Photo**: `images/janakiyamma.jpg`  
   - Size: 300x400 pixels (portrait)
   - Your grandmother's photo

3. **Optional Logo**: `images/logo.png`
   - Size: 200x200 pixels (square)
   - Foundation logo

### Adding Images Process
1. Copy image files to `images/` folder
2. Ensure exact file names match above
3. Commit and push:
   ```bash
   git add .
   git commit -m "Add photos"
   git push
   ```

## 🔧 Advanced Customization

### Adding New Pages
1. Create new `.html` file
2. Copy structure from existing page
3. Add navigation link in all pages:
   ```html
   <li class="nav-item">
       <a href="newpage.html" class="nav-link">New Page</a>
   </li>
   ```

### Adding Interactive Features
- Edit `script.js` for new functionality
- Use existing utility functions
- Follow JavaScript best practices

### SEO Optimization
- Update `<title>` tags in each page
- Add `<meta name="description">` tags
- Use semantic HTML structure

## 🌐 Domain Management

Your domain `janaki-foundation.org` is configured with:
- **DNS Provider**: Namecheap
- **Hosting**: GitHub Pages
- **SSL Certificate**: Automatically provided by GitHub

### DNS Records (Already Set Up)
```
A    @    185.199.108.153
A    @    185.199.109.153  
A    @    185.199.110.153
A    @    185.199.111.153
CNAME www  arunat6543.github.io
TXT   @    v=spf1 include:_spf.google.com ~all
```

## 📱 Testing Your Changes

### Local Testing
1. Open HTML files directly in your browser
2. Check mobile responsiveness (browser dev tools)
3. Test all navigation links

### Live Testing
1. Wait 2-3 minutes after pushing changes
2. Visit [janaki-foundation.org](https://janaki-foundation.org)
3. Test on different devices
4. Clear browser cache if needed (Ctrl+F5)

## 🐛 Troubleshooting

### Common Issues

#### Images Not Loading
- Check file names match exactly (case-sensitive)
- Ensure images are in `images/` folder
- Verify files were committed and pushed

#### Changes Not Appearing
- Wait 2-3 minutes for GitHub Pages deployment
- Clear browser cache (Ctrl+F5)
- Check GitHub repository has your changes

#### Mobile Layout Issues
- Test in browser mobile view (F12 > device toolbar)
- Check CSS media queries in `styles.css`

#### Firebase Not Working
- Verify configuration in `portal.html`
- Check Firebase Console for errors
- Ensure domain is added to authorized domains

## 📞 Getting Help

### Git Commands Quick Reference
```bash
# See current status
git status

# Add all changes
git add .

# Commit with message
git commit -m "Your message here"

# Push to live website
git push

# See recent changes
git log --oneline -5
```

### Useful Resources
- [GitHub Pages Documentation](https://pages.github.com/)
- [Firebase Authentication Guide](https://firebase.google.com/docs/auth)
- [Google Forms Help](https://support.google.com/docs/forms)
- [HTML & CSS Reference](https://www.w3schools.com/)

## 📜 License

This project is created for the Janakiyamma Foundation. All rights reserved.

## 🙏 Acknowledgments

- Built with love in honor of Janakiyamma
- Dedicated to empowering AI/ML education globally
- Starting with a few kids and adults in India

---

**Last Updated**: August 2024  
**Website Version**: 1.0  
**Maintained By**: Janakiyamma Foundation Team

---

## 🚀 Quick Actions

**To edit homepage title:**
1. Open `index.html`
2. Find line 47: `<h1 class="hero-title">`
3. Change the text between the tags
4. Save, commit, and push

**To add grandmother's photo:**
1. Copy photo to `images/janakiyamma.jpg`
2. Run: `git add . && git commit -m "Add grandma photo" && git push`
3. Wait 2 minutes - photo appears on About page

**Need help?** Open an issue in the GitHub repository or refer to the troubleshooting section above.
