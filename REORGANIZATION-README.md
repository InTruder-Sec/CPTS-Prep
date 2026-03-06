# CPTS Exam Preparation Documentation - Reorganization Complete

## Overview

This documentation has been reorganized from a Notion export to follow the ReadMe.com schema and best practices.

## Changes Made

### ✅ Structure Reorganization
- Removed all Notion UUID identifiers from filenames and folder names
- Flattened the nested directory structure for better navigation
- Organized content into logical sections:
  - `general/` - Pentesting methodology, project structure, common tools
  - `information-gathering/` - Reconnaissance and enumeration techniques
  - `tools/` - Essential penetration testing tools (Nmap, RPCclient)
  - `windows/` - Windows-specific enumeration and exploitation
  - `linux/` - Linux-specific techniques (placeholder)
  - `web-exploitation/` - Web application security testing

### ✅ ReadMe.com Compliance
- Added YAML frontmatter to all markdown files with:
  - `title` - Document title
  - `excerpt` - Brief description
  - `hidden` - Visibility setting (false by default)
- Created `_order.yaml` files in each directory to define navigation order
- Fixed internal links to use clean relative paths

### ✅ Content Improvements
- Cleaned up formatting and code blocks
- Improved markdown structure and headers
- Maintained all original content and information
- Added proper table of contents and cross-references

## Directory Structure

```
docs-new/
├── index.md                          # Main landing page
├── _order.yaml                       # Top-level navigation order
├── general/
│   ├── index.md                      # General concepts and methodology
│   ├── ftp.md                        # FTP service enumeration
│   ├── smb.md                        # SMB/CIFS enumeration
│   └── _order.yaml
├── information-gathering/
│   ├── index.md                      # Information gathering overview
│   ├── nmap.md                       # Basic Nmap usage
│   ├── footprinting.md               # Footprinting methodology
│   ├── ssl-enumeration.md            # SSL certificate enumeration
│   ├── cloud-enumeration.md          # Cloud infrastructure discovery
│   └── _order.yaml
├── tools/
│   ├── index.md                      # Tools overview
│   ├── nmap-detailed.md              # Comprehensive Nmap guide
│   ├── rpcclient.md                  # RPCclient usage
│   └── _order.yaml
├── windows/
│   ├── index.md                      # Windows overview
│   ├── general-rdp.md                # Windows basics and RDP
│   ├── os-structure.md               # Windows directory structure
│   ├── file-system.md                # NTFS permissions and shares
│   └── _order.yaml
├── linux/
│   ├── index.md                      # Linux overview (placeholder)
│   └── _order.yaml
└── web-exploitation/
    ├── index.md                      # Web exploitation techniques
    └── _order.yaml
```

## Next Steps

1. **Copy images/assets**: If you have any images referenced in the original docs (like `0-PT-Process-IG.png`), copy them to the appropriate subdirectories
2. **Replace old docs**: Once verified, you can replace the `docs/` folder with `docs-new/`
3. **Update reference folder**: The `reference/` folder already has ReadMe.com config examples - you can keep or customize those
4. **Test on ReadMe.com**: Upload to ReadMe.com to verify the structure works correctly

## Migration Command

When ready to replace the old structure:

```bash
cd /Users/intrud3r/bugbounty/tools/cpts/docs/CPTS-Prep
mv docs docs-old-backup
mv docs-new docs
```

## ReadMe.com Integration

This structure is now ready to be pushed to ReadMe.com. The frontmatter follows their schema:

- Each markdown file has proper metadata
- Navigation order is defined in `_order.yaml` files
- Links are relative and clean
- Content is well-organized and hierarchical

## Notes

- All original content has been preserved
- Notion export artifacts (UUIDs) have been removed
- Internal links have been updated to use clean paths
- The structure is now more maintainable and professional
