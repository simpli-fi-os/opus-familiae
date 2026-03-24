# Opus Familiae

**The Great Work of Family.**

A family of six. Ten ventures. One goal: financial independence for the season that matters most.

Opus Familiae is a content, community, and referral platform for young families pursuing financial independence during the first 15 years of their children's development. Three pillars: **Save. Invest. Earn.**

## What This Is

This repo holds the public-facing Opus Familiae site. Right now it's a single-page countdown tracking the Lott family's debt payoff in real time, with ambient audio and rotating quotes.

The site is part of the [Simpli-FI ecosystem](https://github.com/simpli-fi-os), a network of ten interconnected ventures built by Hunter and Lindsey Lott.

## Project Structure

```
opus-familiae/
  index.html                  Landing page (countdown + ambient audio)
  public/
    assets/
      audio-loop.mp3          Pre-crossfaded seamless loop (27s, production)
      audio.mp3               Original source audio (31s)
  docs/
    Opus-Familiae-CLAUDE.md   Project context and strategy
    Opus-Familiae-White-Paper.docx
    skills/                   Claude skill definitions for OF workflows
```

## Audio Loop Engine

The ambient audio uses a **gapless loop engine** built on the Web Audio API:

1. The source audio was analyzed with `ffmpeg` to map energy levels across the full waveform
2. A 3.5-second equal-power crossfade was applied to blend the tail into the head, producing `audio-loop.mp3`
3. The browser loads this pre-crossfaded file into a Web Audio buffer and schedules overlapping playback with 50ms overlap to eliminate native loop gaps
4. Fade-in on first play, smooth fade-out on pause

No audible seam. No gap. Just continuous atmosphere.

## Local Development

Open `index.html` in a browser. That's it. No build step, no dependencies.

For audio file regeneration (requires `ffmpeg`):

```bash
ffmpeg -i public/assets/audio.mp3 -i public/assets/audio.mp3 \
  -filter_complex \
  "[0:a]atrim=start=27.27:end=30.77,asetpts=PTS-STARTPTS[tail]; \
   [1:a]atrim=start=0:end=3.5,asetpts=PTS-STARTPTS[head]; \
   [tail][head]acrossfade=d=3.5:c1=tri:c2=tri[xfade]; \
   [0:a]atrim=start=3.5:end=27.27,asetpts=PTS-STARTPTS[middle]; \
   [middle][xfade]concat=n=2:v=0:a=1[out]" \
  -map "[out]" -c:a libmp3lame -b:a 192k public/assets/audio-loop.mp3
```

## Deployment

Deployed on [Vercel](https://vercel.com). Push to `main` to deploy.

## The Mission

Every venture traces back to a pain point the Lotts lived. Opus Familiae exists because building financial independence while raising four children shouldn't require a trust fund or a six-figure salary. It requires a plan, a community, and the discipline to execute.

*"As each has received a gift, use it to serve one another, as good stewards of God's varied grace." (1 Peter 4:10, ESV)*

**Save. Invest. Earn. Serve. Build the life. Then live it.**
