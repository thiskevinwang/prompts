---
description: Create a Next.js project with Bun
---

## create-next-app

```zsh
temp_dir="/tmp/nextapp-$(date +%s)"
mkdir -p "$temp_dir"
bunx create-next-app@latest --use-bun --yes "$temp_dir"
mv "$temp_dir"/* ./ 
# include hidden files
mv "$temp_dir"/.* ./ 

# cleanup
rm -rf "$temp_dir"
```
