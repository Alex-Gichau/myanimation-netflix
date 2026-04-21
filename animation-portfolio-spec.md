# Project Specification: Netflix-Style Animation Portfolio

## 1. Project Overview
A web-based portfolio for a professional animator that mirrors the Netflix UI/UX. The site pulls videos from multiple YouTube channels and uses browser storage to create a "Continue Watching" experience without a backend database.

## 2. Core Technical Requirements
- **Frontend Framework:** React.js or Next.js (recommended for state management).
- **Styling:** Tailwind CSS (for the dark-mode grid system).
- **Data Source:** YouTube Data API v3.
- **Persistence:** Browser `localStorage` API.
- **Video Player:** YouTube IFrame Player API.

## 3. Implementation Phases

### Phase 1: Data Architecture & API Integration
The agent must fetch metadata dynamically to ensure view counts and channel names are always current.
- **API Endpoint:** `https://www.googleapis.com/youtube/v3/videos`
- **Required Parts:** `snippet` (for titles, descriptions, and channel names) and `statistics` (for view counts).
- **Task:** Create a helper function `fetchVideoData(videoIds)` that maps an array of IDs to a clean JSON object for the UI.

### Phase 2: User History Engine (Cache Logic)
To "remember" what the user watched, implement a local caching strategy.
- **Schema:** Store an object in `localStorage` named `animation_portfolio_history`.
- **Logic:**
    1. **On Play:** Update the `lastWatchedID` and `timestamp`.
    2. **On Page Load:** Parse the cache. If data exists, inject a "Continue Watching" row at index 0 of the homepage.
    3. **Resumption:** If a user clicks a partially watched video, use the YouTube Player API `player.seekTo(seconds)` to resume.

### Phase 3: The UI/UX Layout (The "Netflix" Look)
The agent should generate components following these design rules:
- **Background:** Deep Black/Grey (`#141414`).
- **Typography:** Sans-serif (Inter or Helvetica), bold headings.
- **The Rows:** - Implement a horizontal slider/carousel.
    - Hover state: Thumbnail scales up (1.1x) and displays an overlay with metadata.
- **The Metadata Bar:** At the bottom of the video description, include:
    - `Channel Name` (Left aligned).
    - `View Count` (Right aligned, formatted as 10K, 1M, etc.).

## 4. Code Snippets for the Agent

### Metadata Component (Description Bottom)
```javascript
const VideoMetadata = ({ channelName, views }) => {
  return (
    <div className="flex justify-between items-center pt-4 border-t border-gray-700 mt-2 text-sm font-semibold text-gray-400">
      <span className="hover:text-white transition-colors">{channelName}</span>
      <span className="bg-gray-800 px-2 py-1 rounded">{views} views</span>
    </div>
  );
}
