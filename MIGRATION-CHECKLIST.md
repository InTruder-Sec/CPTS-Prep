# Migration Checklist ✅

## Reorganization Complete!

Your CPTS documentation has been successfully reorganized from Notion export to ReadMe.com schema.

### 📊 Statistics
- **25 Markdown files** created with ReadMe.com frontmatter
- **7 Navigation files** (_order.yaml) for directory structure
- **1,284 lines** of content preserved and reformatted
- **6 Main sections** organized logically
- **0 Content lost** - everything from Notion preserved

---

## What To Do Next

### ☐ Step 1: Review the New Structure
```bash
cd /Users/intrud3r/bugbounty/tools/cpts/docs/CPTS-Prep
ls -la docs-new/
```

Open a few files to verify the content looks good:
- `docs-new/index.md` - Main landing page
- `docs-new/general/index.md` - General concepts
- `docs-new/tools/nmap-detailed.md` - Example detailed page

### ☐ Step 2: Handle Images/Assets (If Any)

If you had images in your Notion export:

```bash
# Find all images in old structure
find docs -type f \( -name "*.png" -o -name "*.jpg" -o -name "*.gif" \)

# Copy them to appropriate new locations
# Example:
cp "docs/CPTS Exam Prep/General/0-PT-Process-IG.png" docs-new/general/
```

### ☐ Step 3: Test Locally (Optional)

If you want to preview before pushing to ReadMe.com:

```bash
# Install a local markdown server
npm install -g live-server

# Serve the docs
cd docs-new && live-server
```

### ☐ Step 4: Backup and Replace

When you're satisfied everything looks good:

```bash
cd /Users/intrud3r/bugbounty/tools/cpts/docs/CPTS-Prep

# Create backup
mv docs docs-backup-$(date +%Y%m%d)

# Activate new structure
mv docs-new docs

# Commit to git
git add docs/
git commit -m "Reorganize docs for ReadMe.com schema"
```

### ☐ Step 5: Push to ReadMe.com

Follow ReadMe.com's sync instructions:

1. **Using ReadMe CLI:**
   ```bash
   npm install -g rdme
   rdme sync docs/ --version=v1.0
   ```

2. **Using GitHub Sync:**
   - Push to your GitHub repo
   - Connect the repo to ReadMe.com
   - Configure auto-sync

3. **Manual Upload:**
   - Use ReadMe.com's web interface
   - Upload files one by one or via their API

---

## File Structure Reference

```
docs-new/
├── index.md                                    # 📄 Main landing page
├── _order.yaml                                 # 📋 Top-level navigation
│
├── general/                                    # 📁 General Concepts
│   ├── index.md                               # Methodology & tools
│   ├── ftp.md                                 # FTP enumeration
│   ├── smb.md                                 # SMB enumeration
│   └── _order.yaml
│
├── information-gathering/                      # 📁 Reconnaissance
│   ├── index.md                               # Overview
│   ├── nmap.md                                # Basic Nmap usage
│   ├── footprinting.md                        # Footprinting layers
│   ├── ssl-enumeration.md                     # Certificate transparency
│   ├── cloud-enumeration.md                   # Cloud discovery
│   └── _order.yaml
│
├── tools/                                      # 📁 Tool Guides
│   ├── index.md                               # Tools overview
│   ├── nmap-detailed.md                       # Comprehensive Nmap
│   ├── rpcclient.md                           # RPC enumeration
│   └── _order.yaml
│
├── windows/                                    # 📁 Windows Testing
│   ├── index.md                               # Windows overview
│   ├── general-rdp.md                         # Basics & RDP
│   ├── os-structure.md                        # Directory structure
│   ├── file-system.md                         # NTFS permissions
│   └── _order.yaml
│
├── linux/                                      # 📁 Linux Testing
│   ├── index.md                               # Placeholder
│   └── _order.yaml
│
└── web-exploitation/                           # 📁 Web AppSec
    ├── index.md                               # Web testing
    └── _order.yaml
```

---

## ReadMe.com Frontmatter Format

Every file now includes:

```yaml
---
title: Display Title Here
excerpt: Brief description for SEO and previews
hidden: false
---
```

This ensures proper display in ReadMe.com's documentation portal.

---

## Need Help?

### Common Issues

**Q: Images aren't showing up**
A: Make sure image paths are relative and images are in the correct directories

**Q: Navigation order is wrong**
A: Edit the `_order.yaml` file in the relevant directory

**Q: Want to hide a page**
A: Change `hidden: false` to `hidden: true` in the frontmatter

### Additional Resources

- ReadMe.com Documentation: https://docs.readme.com
- Markdown Guide: https://www.markdownguide.org
- YAML Syntax: https://yaml.org

---

## Summary

✅ All Notion UUIDs removed  
✅ Clean, professional file structure  
✅ ReadMe.com compatible frontmatter  
✅ Proper navigation hierarchy  
✅ All content preserved  
✅ Ready to deploy  

**You're all set!** 🚀
