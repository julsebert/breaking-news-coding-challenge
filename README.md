# Coding challenge: breaking news

This project was developed as part of a frontend coding challenge. It demonstrates the implementation of an interactive, draggable Breaking News component using Vue 3, the Composition API, and TypeScript, without additional third-party dependencies.

## User Story 

As a site visitor, I want a repositionable breaking news bubble, so that I can stay updated on urgent news from anywhere on the screen without interrupting my workflow.

### Acceptance Criteria

#### 1. Notification Bubble Behavior

- Displays a numerical badge showing the current count of Breaking News articles.
- Supports free reposition capability (holding the left mouse button and moving the mouse) across the viewport.
- Toggles/opens the "News Item Menu" on click.
  - Clicking does not open the menu when there are zero breaking news.
  - Is keyboard accessible (navigable via Tab and can be triggered via Enter).

#### 2. News Item Menu Behavior

- Is displayed next to the bubble.
- Displays a list of news items showing each article's title.
- Individual news items are highlighted on mouseover.
- Opens the corresponding article in a new browser tab/window upon clicking its title.
- Closes when clicking outside the menu container or pressing the Escape key.

## Out of Scope

- Visual design styling, theme integration, or layout polish.
- Advanced keyboard support with arrow keys.
- The position does not need to be saved, for example on page reload.
- Touch/mobile support.

## Setup & Installation

Requires Node.js 22.18 or newer and npm.

```bash
git clone https://github.com/julsebert/breaking-news-coding-challenge.git
cd breaking-news-coding-challenge
npm install
npm run dev
```

Open the local URL shown in the terminal.

## Mock Data

To use different mock data, change the name in both the import and the `:news` prop in `src/App.vue`. For example, use `breakingNewsMock2` for no news or `breakingNewsMock3` for many. Edit `src/mocks/breakingNews.ts` to add or change items, each needs a `title` and a `link`.

## Testing

The application was manually tested against the acceptance criteria and additional edge cases. Automated testing frameworks were intentionally not introduced to comply with the requirement of using no additional third-party dependencies.

### Acceptance Criteria Testing
All specified acceptance criteria were manually verified.

- Badge displays the correct news count
- Menu opens and closes on click
- Menu does not open when there are no news items
- Bubble can be repositioned across the viewport
- Dragging does not accidentally open the menu
- Menu is displayed next to the bubble
- Menu displays the title of each news item
- News items are highlighted on mouseover
- News links open in a new tab
- Menu closes when clicking outside
- Menu closes when pressing Escape
- Bubble can be focused with Tab and activated with Enter

### Edge Case Testing
Additional edge cases were tested to ensure robust behavior,
including viewport boundaries and empty news lists.

- Menu moves to the left when there is not enough space on the right
- Menu remains inside the viewport near the top and bottom edges
- Long news lists are scrollable
- Bubble remains within the viewport while dragging