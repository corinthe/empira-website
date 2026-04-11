---
name: deploy-ftp
description: Deploy the website to empira.be via FTP
user_invocable: true
---

# Deploy to FTP — empira.be

## Connection

- **Host:** ftp.empira.be
- **Credentials:** stored in `.netrc` at the project root (`C:\empira\new-website\.netrc`)
- **Remote root:** `/` (files are served directly from the FTP root)
- **Subdirectory `bilbio.empira.be`** and `.htaccess` must NOT be touched or deleted

## How to deploy

Use `curl` with the `.netrc` file. Never read or display the `.netrc` contents.

### Upload a single file

```bash
curl --netrc-file /c/empira/new-website/.netrc -T /c/empira/new-website/index.html "ftp://ftp.empira.be/index.html"
```

### Upload to a subdirectory (e.g. css/, js/, img/)

```bash
curl --netrc-file /c/empira/new-website/.netrc -T /c/empira/new-website/css/style.css "ftp://ftp.empira.be/css/style.css"
```

### Full sync procedure

Upload all project files, skipping `.claude/`, `.netrc`, `.git/`, and other non-website files:

```bash
for f in index.html favicon.ico; do
  curl --netrc-file /c/empira/new-website/.netrc -T "/c/empira/new-website/$f" "ftp://ftp.empira.be/$f"
done

for dir in css js img; do
  for f in $(find /c/empira/new-website/$dir -type f); do
    remote="${f#/c/empira/new-website/}"
    curl --netrc-file /c/empira/new-website/.netrc --ftp-create-dirs -T "$f" "ftp://ftp.empira.be/$remote"
  done
done
```

### Test connection

```bash
curl --netrc-file /c/empira/new-website/.netrc -s --list-only "ftp://ftp.empira.be/"
```

## Important

- Never read, display, or log the contents of `.netrc`
- Never delete or overwrite `bilbio.empira.be/` or `.htaccess` on the remote
- Confirm with the user before deploying
