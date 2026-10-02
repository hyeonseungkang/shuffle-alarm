# Shuffle Alarm

Turn a YouTube playlist into an alarm clock. Paste a playlist URL, pick a time, and leave the tab open. When the alarm fires, the next video in the playlist starts playing, so you wake up to a different track each day.

Built with [Next.js](https://nextjs.org/) 12, React 18, and TypeScript. Playlist data comes from the [YouTube Data API v3](https://developers.google.com/youtube/v3).

## How it works

1. Paste a YouTube URL that contains a `list` query parameter. The app extracts the playlist ID and loads the playlist.
2. Set the alarm time. The app shows which video plays next.
3. At the set time, the app embeds that video in an autoplaying YouTube player.
4. The alarm rearms itself for the same time the next day and advances to the following video. After the last video, it wraps back to the first.

Clicking any video in the playlist plays it immediately and makes it the current position.

The playlist ID, alarm time, and last played video are saved in `localStorage`, so they survive a page reload.

## Requirements

- Node.js with npm (Next.js 12.1.6 needs Node.js 12.22 or later)
- A YouTube Data API v3 key from the [Google Cloud Console](https://console.cloud.google.com/apis/library/youtube.googleapis.com)

## Setup

Create a `.env` file in the project root:

```
YOUTUBE_DATA_API_KEY=your_api_key
```

Install dependencies:

```bash
npm install
```

`npm install` also runs a production build through the `postinstall` script, so set the API key first.

## Running

Development server with hot reload:

```bash
npm run dev
```

Production:

```bash
npm run build
npm start
```

The app is served at [http://localhost:3000](http://localhost:3000).

## Scripts

| Script          | Description                                |
| --------------- | ------------------------------------------ |
| `npm run dev`   | Start the development server               |
| `npm run build` | Remove `.next` and create a production build |
| `npm start`     | Serve the production build                 |
| `npm run lint`  | Run ESLint                                 |

## API

### `GET /api/playlist?playlistId=<id>`

Proxies the YouTube Data API `playlistItems` endpoint, which keeps the API key on the server. Returns the YouTube response as JSON, with `items` holding the videos.

- Missing `playlistId`: `404` with the text `playlistId not found`.
- Request to YouTube failed: `200` with the text `invalid playlistId`.

## Project structure

```
components/
  Header.tsx          Title and icons
  Footer.tsx          Footer links
pages/
  index.tsx           Main page: URL input, alarm, player, playlist
  api/playlist.ts     YouTube Data API proxy
public/               Icons and favicon
styles/               Global and page CSS
```

## Limitations

- The tab must stay open and the device awake. The alarm is a browser timer, not a system alarm.
- Browsers may block autoplay with sound until you have interacted with the page. Click a video once to test playback before relying on the alarm.
- Only the first 50 videos of a playlist are loaded.
- Private playlists are not supported.
- Videos play in playlist order. There is no random shuffle.
