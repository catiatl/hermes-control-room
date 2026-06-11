# OmniGet / yt-dlp — Téléchargement vidéo multi-plateforme

## Utilité
Télécharge des vidéos depuis X/Twitter, Instagram, TikTok, YouTube, Reddit, etc.
Basé sur yt-dlp (déjà installé).

## Commandes
```bash
# X/Twitter
yt-dlp -o "%(title).50s.%(ext)s" "https://x.com/user/status/ID"

# Meilleure qualité
yt-dlp -f "best[height<=720]" -o "video.mp4" "URL"

# Compresser pour Telegram (limite 20MB)
ffmpeg -i input.mp4 -vf "scale=-2:360" -vcodec libx264 -crf 32 -preset fast -acodec aac -b:a 32k output.mp4

# Extraire audio pour transcription
ffmpeg -i input.mp4 -vn -acodec pcm_s16le -ar 16000 -ac 1 audio.wav
```

## Usage content
- Télécharger des vidéos tendances pour inspiration TikTok
- Récupérer des références pour montage
- Transcriptions via whisper (déjà installé)

## Installation
yt-dlp déjà installé. OmniGet est une app desktop (GUI) qui wrappe yt-dlp.
