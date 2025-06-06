# Mobile Apps


Over the years, I’ve developed and published a number of mobile apps — primarily for **Apple iOS**, though I’ve also ventured into **Android** using **Java** and **Kotlin**. This post offers a curated list of those projects that I believe are most worth sharing.

Some of these apps are **open source**, and I’ve linked to their public repositories where available — feel free to explore, fork, or contribute.

> What we now call “apps” began as simple, standalone tools. Here are mine — crafted from scratch, designed with care, and sometimes a bit of eccentric joy.

{{< admonition type=tip >}}
Unless otherwise noted, I was responsible for:
- UI/UX and graphic design  
- Core development and architecture  
- Music composition or arrangement  
- Promotional video production  
- ... and nearly everything else.
{{< /admonition >}}

---

## ANTOINE: *that* little Prince 🤴

![](antoine-appicon.png)

An experimental text reader app based on *The Little Prince* by Antoine de Saint-Exupéry. “Antoine” reimagines the reading experience with an **interactive, multilingual, and sensor-enhanced** storytelling interface.

### ✨ Features
- Includes the full text of *The Little Prince* in **four different languages**, switchable interactively.
- Every paragraph is **touch-sensitive**: tap once to switch translations in a circular sequence, or toggle between a preferred pair of languages.
- Each language has its **own font size and color settings**, individually customizable.
- Embedded **illustrations come alive** with subtle parallax and **gyroscopic** motion effects.
- A hidden **side-scrolling arcade mini-game** transforms the illustrations into enemies and bonuses — set against dynamic music that evolves with gameplay intensity.
- Soundtrack and effects are context-sensitive, dynamically responding to in-game events.

{{< vimeo 211160209 >}}

> 🎵 Music composed and performed by Christian Cellini. Used with permission.

---

## CHATEA Messaging App

![](chatea-appicon.png)

![License](https://img.shields.io/github/license/lucaji/chatea)
![GitHub stars](https://img.shields.io/github/stars/lucaji/chatea)
![GitHub forks](https://img.shields.io/github/forks/lucaji/chatea)

A **peer-to-peer messaging app** for nearby devices using Bluetooth and Wi-Fi, **without the need for internet or external servers**. Built primarily for iOS as a lightweight chat utility with rich media features.

### ✨ Features
- Real-time **text chat with emoji** support
- **Photo sharing** and previews
- **Hand-drawn sketches** using touch gestures
- **Push-to-talk (PTT)** voice messaging for walkie-talkie style communication

## 🔗 iOS [Source Code](https://github.com/lucaji/chatea)

> The name *Chatea* is a blend of “Chat” and “Tea” — with a nod to the Mandarin word “Cha” (茶), meaning tea. The app’s logo also reflects this ideogram.

---

## MYTUNA: Tunings Librarian

![](mytuna-appicon.png)

![License](https://img.shields.io/github/license/lucaji/MyTuna)
![GitHub stars](https://img.shields.io/github/stars/lucaji/MyTuna)
![GitHub forks](https://img.shields.io/github/forks/lucaji/MyTuna)

A highly practical **tuning preset organizer** for musicians working with stringed instruments. Designed with flexibility and playback features in mind.

### ✨ Features
- Save and label **custom tunings** for any number of strings
- Generate **sine wave tones** for each string
- Play full tuning sets via **arpeggiated playback** or **strumming simulation**
- Export your library to **CSV** (compatible with Excel and Numbers)
- Optional **pitch detection** and **reference tone playback**
- Full **iCloud Drive support** for backup and sharing

{{< figure src="mytuna05.jpg" title="MYTUNA tunings librarian" >}}

## 🔗 iOS [Source Code](https://github.com/lucaji/mytuna)

---

## PIANOTONER (aka ToneTuna)

![](tonetuna-appicon.png)

A **precision tone generator** built around a **fully playable piano interface**, covering the standard 88-key range. Ideal for tuning pianos, synths, and analog instruments by ear.

### ✨ Features
- Interactive **keyboard interface**
- Generates accurate **sine wave tones** for all 88 keys
- Adjust the **reference pitch of A4** to explore different temperaments
- Ultra-low-latency output ideal for tuning analog gear

{{< figure src="tonetuna01.jpg" title="ToneTuna: sine wave piano tone generator" >}}

## 🔗 iOS [Source Code](https://github.com/lucaji/pianotoner_ios)  
## 🔗 Android [Source Code](https://github.com/lucaji/pianotoner_android)

![License](https://img.shields.io/github/license/lucaji/pianotoner_ios)
![GitHub stars](https://img.shields.io/github/stars/lucaji/pianotoner_ios)
![GitHub forks](https://img.shields.io/github/forks/lucaji/pianotoner_ios)

---

## SHARI: PDF Reader with Web Server

![License](https://img.shields.io/github/license/lucaji/Shari)
![GitHub stars](https://img.shields.io/github/stars/lucaji/Shari)
![GitHub forks](https://img.shields.io/github/forks/lucaji/Shari)

A simple but elegant **PDF document reader** with built-in **WebDAV server** support — enabling file uploads/downloads without cloud storage.

### ✨ Features
- Smooth and responsive **PDF viewing experience**
- Built-in **WebDAV file server** (connect via Wi-Fi browser or Finder)
- Manage documents directly from your laptop or desktop
- Perfect for air-gapped devices or local-only workflows

## 🔗 iOS [Source Code](https://github.com/lucaji/Shari)

---

## STARTREC: Multicam Timelapse and Sync App

![](startrec-appicon.png)

A robust, experimental **multi-camera app** supporting synchronized audio/video/timelapse recording across multiple iOS devices connected via local network.

> 🚧 This project is currently on hold due to time constraints.

### ✨ Features
- **Timelapse**, **photo**, **audio**, and **video** capture
- On-device **media gallery** and **GIF generator**
- Advanced **timing and synchronization** tools for multicam environments
- Built-in **timelapse calculator** for real-time planning
- Customizable **manual camera controls** with live focus/exposure gestures
- **iCloud Drive** support for media storage

### 🧮 Timelapse Calculator

![](startrec03.jpg)

You can either:
- Enter the interval time, number of takes, and desired FPS, **or**
- Input the total shoot duration, interval, FPS, and playback length.

The app calculates the required recording time and resulting playback duration.

![](startrec05.jpg)

### ⏱️ Timed Recordings

The app runs two timers during automated sessions:
- A **preroll timer** to prep before capture starts
- An **interval timer** that manages spacing between takes

For each take, the app automatically triggers `start` and `stop` based on your mode (photo, video, or audio).

### 📷 Manual Camera Controls

![](startrec04.jpg)

Enable **Touchy mode** to set focus and exposure dynamically by dragging across the preview area.

### 🎞️ Videos

- Promo Video:  
  {{< vimeo 211155832 >}}

- Touch Live Controls:  
  {{< vimeo 211154409 >}}

- Live Session Footage:  
  {{< vimeo 211158581 >}}

> 🎵 Music by Christian Cellini. Used with permission.

