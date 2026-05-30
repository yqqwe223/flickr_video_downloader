# Flickr Video Downloader - README

> Lightweight tool to extract and save public videos from Flickr. Built for personal use, learning, and technical research. Please respect platform terms of service and applicable laws in your region.

🔗 Live demo: [https://twittervideodownloaderx.com/flickr_downloader](https://twittervideodownloaderx.com/flickr_downloader)

---

## 📌 Why I built this

Let's be real – when you're browsing Flickr for visual inspiration, you sometimes stumble across really solid videos. Landscape timelapses, behind-the-scenes from photo shoots, short travel vlogs that tell a story better than a single image... But then you hit the wall: no download button. Bookmark it and forget? Happens to everyone. Screen record? Quality drops, takes forever, and you end up with a huge file.

So I thought, "Why not just build something simple that gets the job done?" That's how this project started. No bloat, no fancy UI, just: paste link → get video. Code is kept clean, dependencies are minimal, and deployment shouldn't give you a headache. If it helps you too, great. If you want to peek under the hood or improve it, even better.

---

## ✨ What it does

- ✅ Parses public Flickr video links (supports album embeds, profile pages, shared links, etc.)
- ✅ Auto-detects multiple resolutions and prioritizes original quality when available
- ✅ Backend handles the heavy lifting; frontend is just a simple form → fast load, zero tracking scripts
- ✅ CORS pre-configured for easy integration with other frontend projects
- ✅ Basic request logging + parse status for quicker debugging
- ✅ Built-in rate limiting to reduce the chance of getting blocked by Flickr

---

## 🛠️ Tech stack

- Language: Python 3.9+
- Framework: Django 4.x (lightweight, easy to extend)
- HTTP client: requests (main), httpx (optional for async mode)
- Parsing: regex + BeautifulSoup (when needed)
- Deployment: Gunicorn + Nginx recommended; Docker support for quick setup
- Config management: environment variables + settings.py split by environment (dev/prod)

External dependencies are kept to a minimum to avoid version conflicts and make installation smoother.

---

## 🚀 Quick start

### Option 1: Run from source

```bash
# 1. Clone the repo
git clone https://github.com/yourname/flickr-downloader.git
cd flickr-downloader

# 2. Create virtual env and install dependencies
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt

# 3. Copy and edit config file
cp .env.example .env
# Edit .env: set SECRET_KEY, ALLOWED_HOSTS, and other required values

# 4. Run migrations (if you want to log requests to DB)
python manage.py migrate

# 5. Start the dev server
python manage.py runserver 0.0.0.0:8000

# For production, use Gunicorn:
gunicorn core.wsgi:application --bind 0.0.0.0:8000 --workers 3
```

### Option 2: Docker (for the lazy)

```bash
# Build the image
docker build -t flickr-dl:latest .

# Run the container
docker run -d -p 8000:8000 --env-file .env flickr-dl:latest
```

> 💡 Pro tip: When deploying to production, always set up Nginx as a reverse proxy and enforce HTTPS. Security isn't optional.

---

## 📋 API usage example

```bash
# Test with curl
curl -X POST https://your-domain.com/api/parse \
  -H "Content-Type: application/json" \
  -d '{"url": "https://www.flickr.com/photos/xxx/video/12345678"}'

# Expected JSON response
{
  "code": 200,
  "data": {
    "title": "Sunrise timelapse at Bromo",
    "author": "Alex Photographer",
    "video_url": "https://cdn.flickr.com/video/xxx.mp4",
    "thumbnail": "https://cdn.flickr.com/thumb/xxx.jpg",
    "duration": "03:45"
  }
}
```

The web interface comes with a barebones form: paste link → click "Parse" → get download button. Three steps, done. No fluff.

---

## ⚠️ Important notes & disclaimer

1. This tool only works with **publicly available** Flickr videos. Content requiring login or set to private won't be processed;
2. Do NOT use for mass scraping, commercial redistribution, or any action that violates Flickr's Terms of Service;
3. Video copyrights belong to the original creator or platform. Please use downloaded content for personal study, research, or fair citation only;
4. Sending too many requests in a short time may trigger rate limiting. The code includes a basic delay mechanism – please enable it;
5. This project does NOT store any video files. Returned links are direct from Flickr's official CDN and may expire according to platform policies;
6. The developer assumes no legal or technical liability for issues arising from the use of this tool. Use at your own risk.

---

## 🤝 Contributing

Bug reports, feature suggestions, and PRs are welcome. Before submitting, please:

- Clearly describe how to reproduce the issue, including the problematic link and exact error message;
- New features should have general applicability – avoid overly specific customizations;
- Keep code style consistent with the existing codebase (PEP8 + project .editorconfig);
- If modifying parsing logic, please add test cases to prevent regressions.

Small fixes: feel free to PR directly. Larger changes: open an Issue first to discuss approach and save everyone time.

---

## 📄 License

MIT License ©   
You're free to use, modify, and distribute this code, as long as you retain attribution to the original author. For commercial use, please verify compliance with applicable laws and regulations on your own.

---

> 🌱 Full disclosure: I built this primarily for my own needs, so it's definitely not perfect. If you run into "parse failed" errors or broken links, chances are Flickr updated their page structure or anti-bot measures. Drop an Issue and I'll take a look when I can – or if you're comfortable with code, give fixing it a shot yourself. Sometimes the best way to learn a system is to get your hands dirty debugging it.  
>   
> One last thing, and I mean this sincerely: respect the creators who put effort into making content, and use tools like this responsibly. That's the only way we keep useful projects like this alive and available for everyone. Thanks for checking out this repo – hope this little tool saves you some time or helps with your work/studies! 🙏✨

---

## 🔧 Troubleshooting tips

- **Parse fails suddenly**: Flickr likely changed their HTML structure. Check recent Issues or pull the latest code.
- **403/429 errors**: You're hitting rate limits. Enable the request delay in settings or reduce concurrent requests.
- **Video URL expires quickly**: This is normal – Flickr CDN links have short TTLs. Download promptly after parsing.
- **Docker won't start**: Double-check your .env file syntax and ensure port 8000 isn't already in use.

---

## 📦 Project structure (simplified)

```
flickr-downloader/
├── core/               # Django project config
├── parser/             # Video extraction logic
├── static/             # Minimal frontend assets
├── templates/          # HTML templates
├── .env.example        # Config template
├── requirements.txt    # Python dependencies
├── Dockerfile          # Container build instructions
└── README.md           # You are here
```

Keep it simple, keep it maintainable. That's the philosophy.

---

> Last word: if this tool helped you out, cool. If you improved it, even cooler. Share knowledge, stay curious, and happy coding. 🚀