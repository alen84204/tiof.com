# TIOF CMS Data Schema Definition

This document defines a comprehensive JSON data schema for the Taiwan International Ocean Forum (TIOF). The schema is designed for a headless CMS environment (such as Sanity, Strapi, or Contentful), emphasizing a modular, component-based architecture. It facilitates annual content updates through a year-based object structure and provides full support for bilingual (ZH/EN) content delivery.

## 1. Schema Architecture Overview

The schema is organized into logical modules that correspond to the frontend UI components. Each text-heavy field uses a localized object structure to ensure consistency across the application.

### 1.1 Localized String Schema
To manage bilingual requirements, all string fields follow this pattern:
```json
{
  "zh": "中文內容",
  "en": "English content"
}
```

## 2. Global Site Settings & Navigation

Global settings manage the persistent elements of the site, such as SEO, identity, and the main navigation menu.

| Field | Type | Description |
| :--- | :--- | :--- |
| `siteName` | Localized String | The official name of the forum. |
| `logo` | Image Object | SVG or PNG assets for the header and footer. |
| `seo` | Object | Contains `title`, `description`, and `ogImage` for social sharing. |
| `socialLinks` | Array | List of platforms (Facebook, X, LinkedIn) and their URLs. |
| `navMenu` | Array | Navigation items with labels and anchor links/routes. |

## 3. Annual Forum Content Schema

The main content is encapsulated within a year-specific object to allow for easy archiving and future-proofing (e.g., transitioning from 2025 to 2026).

### 3.1 Hero Section
- **mainTitle**: Localized string for the primary heading.
- **subtitle**: Localized string for the secondary heading.
- **forumTheme**: Localized string for the annual specific theme.
- **eventDetails**: Date and Location strings.
- **ctaButtons**: Array of objects containing `label`, `link`, and `style` (primary/secondary).
- **heroVisual**: Object containing either a background image URL or a video stream link.

### 3.2 About & Pillars
- **aboutContent**: Localized long-form text (Markdown supported).
- **coreValues**: Array of 2-4 cards, each with a localized title and description.
- **themePillars**: A configurable collection of 3-4 items.
    - `title`: Localized.
    - `description`: Localized short text.
    - `icon`: Asset reference.
    - `sortOrder`: Integer for frontend display logic.

### 3.3 Speakers & Agenda
- **Speakers**:
    - `id`: Unique identifier.
    - `name`: Localized.
    - `title`: Localized position/rank.
    - `institution`: Localized organization name.
    - `bio`: Localized detailed biography.
    - `photo`: Image URL.
    - `isVisible`: Boolean toggle for publishing.
- **Agenda**:
    - `days`: Array of "Day" objects (e.g., Day 1, Day 2).
    - `sessions`: Array of session objects linked to a specific day.
        - `time`: Start and end time strings.
        - `title`: Localized session name.
        - `description`: Localized summary.
        - `moderator`: Reference to Speaker ID or string.
        - `speakers`: Array of Speaker ID references.
        - `isFeatured`: Boolean for homepage preview.

### 3.4 Media & Partners
- **Media Resources**: Category-based items (News, Videos, PDF Results).
- **Partner Wall**: Categorized by "Host", "Organizer", "Partner", or "Supporter". Each contains a logo, name, and external link.

---

## 4. Sample 2026 Data Object

The following JSON object demonstrates how the schema would be populated for the 2026 forum cycle.

```json
{
  "year": 2026,
  "isActive": true,
  "hero": {
    "mainTitle": {
      "zh": "2026 台灣國際海洋論壇",
      "en": "2026 Taiwan International Ocean Forum"
    },
    "subtitle": {
      "zh": "創新科技與海洋永續的交匯",
      "en": "Intersection of Innovation and Ocean Sustainability"
    },
    "forumTheme": {
      "zh": "智造藍色新未來",
      "en": "Smart Manufacturing for a Blue Future"
    },
    "date": "2026-06-08",
    "location": {
      "zh": "高雄萬豪酒店",
      "en": "Kaohsiung Marriott Hotel"
    },
    "cta": [
      {
        "label": { "zh": "立即報名", "en": "Register Now" },
        "url": "/register",
        "type": "primary"
      },
      {
        "label": { "zh": "查看議程", "en": "View Agenda" },
        "url": "#agenda",
        "type": "secondary"
      }
    ]
  },
  "about": {
    "content": {
      "zh": "TIOF 2026 將匯聚全球頂尖專家，探討人工智慧在海洋治理中的角色...",
      "en": "TIOF 2026 gathers global experts to explore the role of AI in ocean governance..."
    },
    "pillars": [
      {
        "id": "p1",
        "title": { "zh": "海域安全", "en": "Maritime Security" },
        "description": { "zh": "強化自動化監測系統", "en": "Enhancing autonomous monitoring" },
        "sortOrder": 1
      },
      {
        "id": "p2",
        "title": { "zh": "藍色經濟", "en": "Blue Economy" },
        "description": { "zh": "永續能源的開發", "en": "Developing sustainable energy" },
        "sortOrder": 2
      }
    ]
  },
  "speakers": [
    {
      "id": "sp-001",
      "name": { "zh": "陳教授", "en": "Prof. Chen" },
      "title": { "zh": "首席科學家", "en": "Chief Scientist" },
      "institution": { "zh": "國家海洋研究院", "en": "NAMR" },
      "bio": { "zh": "專精於海洋大數據分析...", "en": "Expert in marine big data..." },
      "photo": "https://example.com/photos/chen.jpg",
      "isVisible": true,
      "sortOrder": 1
    }
  ],
  "agenda": {
    "days": [
      {
        "dayNumber": 1,
        "date": "2026-06-08",
        "sessions": [
          {
            "time": "09:00 - 10:00",
            "title": { "zh": "開幕式", "en": "Opening Ceremony" },
            "type": "keynote",
            "speakerRefs": ["sp-001"],
            "showOnHome": true
          }
        ]
      }
    ]
  },
  "partners": [
    {
      "category": "host",
      "name": { "zh": "海洋委員會", "en": "Ocean Affairs Council" },
      "logo": "https://example.com/logos/oac.png",
      "link": "https://www.oac.gov.tw",
      "sortOrder": 1
    }
  ]
}
```

## 5. Implementation Notes

### 5.1 Sorting and Ordering
All arrays containing content intended for the landing page (pillars, speakers, partners) include a `sortOrder` field. This allows the CMS editor to control the visual flow of the website without requiring technical adjustments.

### 5.2 Dynamic Year Handling
The frontend application should be configured to fetch data filtered by the `year` field and the `isActive` boolean. This allows administrators to prepare the 2027 content in the background while the 2026 site is still live.

### 5.3 Reference Integrity
The Agenda module uses `speakerRefs` (IDs) rather than duplicating speaker data. This ensures that if a speaker's title or photo is updated in the Speaker module, the change reflects automatically across all sessions in the Agenda.