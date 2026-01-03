# Claude Code Mobile

Run Claude Code from your iPhone or iPad. Code from anywhere.

![Claude Code Mobile](https://img.shields.io/badge/Claude%20Code-Mobile-blue)
![Platform](https://img.shields.io/badge/Platform-iOS%20%7C%20macOS-lightgrey)

---

## Why This Is Awesome

Imagine these scenarios:

- **Stuck in traffic?** Fix that bug from the back of an Uber
- **Coffee shop coding** — no laptop needed, just your phone
- **Quick deployments** while walking between meetings
- **Review and approve** Claude's changes from bed at midnight
- **Pair program with AI** from literally anywhere with internet
- **Monitor long-running tasks** while away from your desk

Claude Code's natural language interface is perfect for mobile. You're not typing code character-by-character — you're having a conversation. Describe what you want, review changes, approve or reject. That works great on a phone.

---

## What You'll Set Up

```
┌──────────────┐         SSH          ┌──────────────┐
│   iPhone     │ ──────────────────▶  │     Mac      │
│  (Termius)   │                      │ (Claude Code)│
└──────────────┘                      └──────────────┘
```

Your phone connects to your Mac via SSH. You run Claude Code on your Mac, but control it from your phone.

**Total setup time: ~10 minutes**

---

## Step 1: Enable SSH on Your Mac

SSH lets your phone securely connect to your Mac's terminal.

### Option A: System Settings (Easiest)

1. Open **System Settings**
2. Go to **General** → **Sharing**
3. Toggle **Remote Login** → **ON**
4. Set "Allow access for" to **All users** (or just yourself)

### Option B: Terminal Command

```bash
sudo systemsetup -setremotelogin on
```

Enter your Mac password when prompted.

### Verify It's Working

```bash
nc -z localhost 22 && echo "SSH is running!"
```

---

## Step 2: Get Your Mac's IP Address

Run this in Terminal:

```bash
ipconfig getifaddr en0
```

You'll get something like `192.168.1.175`. Save this — you'll need it.

**Note:** This IP only works on your home Wi-Fi. For access from anywhere, see the [Tailscale section](#optional-access-from-anywhere-with-tailscale) below.

---

## Step 3: Install Termius on iPhone

[Termius](https://apps.apple.com/us/app/termius-modern-ssh-client/id549039908) is a free SSH app for iOS.

1. Download **Termius** from the App Store
2. Open the app
3. Tap the **Connections** tab (bottom)
4. In the search bar at top, type:
   ```
   yourusername@your-mac-ip
   ```
   Example: `john@192.168.1.175`
5. Tap **CONNECT**
6. Enter your Mac login password
7. You're in!

### Save It For Quick Access

1. Tap the **+** button in Termius
2. Enter your IP as the hostname
3. Enter your username
4. Save — now it's one tap to connect

---

## Step 4: Run Claude Code

Once connected via SSH, just type:

```bash
claude
```

That's it. You're now running Claude Code from your phone.

---

## Step 5: Install tmux (Keep Sessions Alive)

**Problem:** iOS kills background apps after ~3 minutes. If you switch apps or lock your phone, your SSH session dies and Claude Code stops.

**Solution:** tmux keeps your session running on your Mac even when you disconnect.

### Install tmux

On your Mac:

```bash
brew install tmux
```

### Start Claude Code in tmux

From your phone (via Termius):

```bash
tmux new -s claude
claude
```

### Reconnect After Disconnecting

If your session drops, just run:

```bash
tmux attach -s claude
```

You're right back where you left off. Claude Code kept running the whole time.

### tmux Cheat Sheet

| Action | Command |
|--------|---------|
| Start new session | `tmux new -s claude` |
| Detach (leave running) | `Ctrl+B` then `D` |
| Reattach | `tmux attach -s claude` |
| List sessions | `tmux ls` |
| Kill session | `tmux kill-session -s claude` |

---

## Optional: Access From Anywhere with Tailscale

By default, SSH only works on your local Wi-Fi network. To use Claude Code from anywhere (coffee shop, 5G, traveling), use [Tailscale](https://tailscale.com).

Tailscale creates a secure private network between your devices. It's free for personal use.

### Setup

1. Install Tailscale on your Mac: https://tailscale.com/download/mac
2. Install Tailscale on your iPhone: App Store
3. Sign in with the same account on both
4. Your Mac gets a Tailscale IP (like `100.x.x.x`)
5. Use that IP in Termius instead of your local IP

Now you can SSH into your Mac from anywhere in the world.

---

## Tips & Tricks

### Voice Input

Enable dictation on your iPhone keyboard. You can literally talk to Claude Code:

> "Create a function that validates email addresses and returns true or false"

Claude Code's natural language interface makes this surprisingly effective.

### Save Common Commands

In Termius, save snippets for commands you use often:

- `tmux attach -s claude` — reconnect to Claude
- `claude --resume` — resume last conversation
- `cd ~/projects/myapp && tmux new -s claude` — start in project directory

### Bluetooth Keyboard

For longer sessions, connect a Bluetooth keyboard to your iPhone. Game changer.

### iPad Split View

On iPad, use Split View to have Termius and Safari side-by-side. Research while coding.

---

## Current Limitations

### iOS Background Restrictions
iOS suspends apps after ~3 minutes in background. **Workaround:** Use tmux (covered above).

### Typing Speed
Phone keyboards are slower than real keyboards. **Workaround:** Use voice input, save snippets, or connect a Bluetooth keyboard.

### Screen Size
You're working on a small screen. **Workaround:** Claude Code's conversation-based interface actually works well on mobile since you're not reading huge files — you're chatting.

### Local Network Only (by default)
SSH only works on your home Wi-Fi unless you set up Tailscale or port forwarding. **Workaround:** Use Tailscale (free, 5-minute setup).

### No Native iOS App (Yet)
As of early 2025, there's no official Claude Code iOS app. You're running it through SSH. Anthropic may release a native app in the future.

### Battery Usage
Long SSH sessions with active terminal output can drain your phone battery. **Workaround:** Keep a charger handy for long sessions.

---

## Troubleshooting

### "Connection refused" error
- Make sure Remote Login is enabled on your Mac
- Verify your IP address is correct (`ipconfig getifaddr en0`)
- Check you're on the same Wi-Fi network

### "Permission denied" error
- Double-check your username (run `whoami` on your Mac)
- Make sure you're typing your Mac login password correctly

### Session dies when I switch apps
- Use tmux! See [Step 5](#step-5-install-tmux-keep-sessions-alive)

### Can't connect from outside home
- Set up Tailscale. See [Access From Anywhere](#optional-access-from-anywhere-with-tailscale)

---

## Requirements

- **Mac** with macOS (any recent version)
- **iPhone** or iPad with iOS 15+
- **Claude Code** installed on your Mac (`npm install -g @anthropic-ai/claude-code`)
- **Termius** app (free on App Store)
- Same Wi-Fi network (unless using Tailscale)

---

## Contributing

Found a better way to do something? Open a PR!

## License

MIT — do whatever you want with this.

---

## 📱 Mobile Quick Reference (Save This!)

### Daily Workflow

**First Connection:**
```bash
# Connect via Termius using your Tailscale IP
ssh yourusername@100.x.x.x

# Start tmux session
tmux new -s claude

# Launch Claude Code
claude
```

**Disconnecting (Without Killing Claude):**
```
Ctrl+B, then D
```
☝️ This detaches but keeps everything running on your Mac

**Reconnecting Later:**
```bash
# Connect via Termius again
ssh yourusername@100.x.x.x

# Reattach to your session
tmux attach -t claude
```
You're right back where you left off!

---

### Essential Commands

| What You Want | Type This |
|--------------|-----------|
| Start new tmux session | `tmux new -s claude` |
| Pause & keep running | `Ctrl+B` then `D` |
| Resume session | `tmux attach -t claude` |
| See all sessions | `tmux ls` |
| Kill session completely | `tmux kill-session -t claude` |
| Start Claude Code | `claude` |
| Exit Claude Code | Type `exit` or `/exit` |

---

### Your Connection Info

**Local Network Only:**
```bash
# Your Mac's local IP (home WiFi only)
ipconfig getifaddr en0
```

**From Anywhere (Tailscale):**
```bash
# Your Mac's Tailscale IP (works everywhere)
/Applications/Tailscale.app/Contents/MacOS/Tailscale ip -4
```

In Termius, use your **Tailscale IP** (`100.x.x.x`) to connect from anywhere.

---

### Switching Between Phone & Desktop

The tmux session runs **on your Mac**, so you can access it from anywhere:

**On Phone (Termius):**
```bash
tmux attach -t claude
# work work work
Ctrl+B D  # detach
```

**Later on Desktop (Terminal):**
```bash
tmux attach -t claude
# continue exactly where you left off
```

You can even attach from **both at the same time** - they show the same thing in real-time!

---

### How tmux Saves Your Butt

| Without tmux | With tmux |
|--------------|-----------|
| Signal drops → Claude dies | Signal drops → Claude keeps running |
| Switch apps → Session lost | Switch apps → Come back anytime |
| Have to restart every time | Pick up exactly where you left off |
| Long tasks interrupted | Tasks complete even when disconnected |

**Rule of thumb:** Always start Claude Code inside tmux on mobile.

---

### Quick Troubleshooting

**"Session not found"**
- You haven't started a tmux session yet
- Run: `tmux new -s claude`

**"Can't connect"**
- On local WiFi? Use your local IP
- From outside? Use Tailscale IP (`100.x.x.x`)
- Make sure Tailscale is running on both devices

**"Session died anyway"**
- You probably didn't start Claude inside tmux
- Always: `tmux new -s claude` → THEN → `claude`

**"Everything is frozen"**
- Try: `Ctrl+C` to cancel current operation
- Last resort: `Ctrl+B D` to detach, then reattach
