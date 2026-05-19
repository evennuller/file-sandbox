# 📥 file-sandbox

A GitHub Actions workflow that downloads any file from the internet simply by pasting a URL in your commit message. Powered by **aria2c**, **wget**, or **curl** — your choice.

No servers. No dashboards. Just commit and download.

> [!NOTE]
> This project was built with the assistance of Claude (Anthropic). The code, structure, and documentation were generated through an AI-assisted development session and reviewed by the author.

---

## ✨ How it works

1. You push a commit with a URL in the message
2. GitHub Actions picks it up and downloads the file
3. The file lands in the `downloads/` folder of your repo
4. If the file is over 90 MB, it's automatically split into `.zip` chunks

---

## 🚀 Quick start

### 1. Fork or create a repo

Create a new GitHub repository (private recommended — your downloads will live here).

### 2. Add the workflow

Create the file `.github/workflows/download-files.yml` and paste in the workflow contents.

### 3. Allow Actions to push

Go to **Settings → Actions → General → Workflow permissions** and enable **Read and write permissions**.

### 4. Commit a URL

Edit any file in the repo (e.g. `README.md`), and use a URL as your commit message:

```
https://example.com/file.zip
```

That's it. Check the **Actions** tab and watch it run.

---

## ⚙️ Options

Add any of these lines to your commit message to customise the download:

| Option         | Example                 | Default                |
| -------------- | ----------------------- | ---------------------- |
| `tool:`        | `tool: wget`            | `aria2c`               |
| `filename:`    | `filename: my-file.zip` | auto-detected from URL |
| `connections:` | `connections: 16`       | `8`                    |
| `retries:`     | `retries: 5`            | `10`                   |

### Available tools

| Tool     | Best for                                   |
| -------- | ------------------------------------------ |
| `aria2c` | Large files, fast multi-threaded downloads |
| `wget`   | Standard HTTP/FTP, resume support          |
| `curl`   | Redirect-heavy or tricky URLs              |

---

## 📝 Commit message examples

**Basic download:**

```
https://speed.hetzner.de/100MB.bin
```

**Custom filename + faster connections:**

```
https://releases.ubuntu.com/24.04/ubuntu-24.04-desktop-amd64.iso
filename: ubuntu-24.04.iso
connections: 16
```

**Use wget with retries:**

```
https://example.com/archive.tar.gz
tool: wget
retries: 3
```

---

## 📦 Large file handling

Files over **90 MB** are automatically split into multipart `.zip` archives (to stay within GitHub's file size limits) and placed in a named subfolder under `downloads/`.

To reassemble them locally:

```bash
zip -s 0 merged.zip --out single.zip
unzip single.zip
```

---

## ⚠️ Limitations

- GitHub has a **1 GB soft limit** on repository size — clean up `downloads/` regularly for heavy use
- GitHub Actions has a **6-hour job timeout** — very large files may not finish in time
- Only **direct URLs** work (no login-walled or JavaScript-rendered pages)
- This workflow is intended for **personal use** — be mindful of the terms of service of any site you download from

---

## 📄 License

MIT — do whatever you want with it, okay????
