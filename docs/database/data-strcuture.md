# Music Database Structure for Recommendation System

## 📊 **DATABASE TABLES OVERVIEW**

### **Main Entities Relationship:**
```
ARTIST (1) -----> (Many) ALBUMS -----> (Many) TRACKS -----> (Many) SECTIONS
   |                                        |
   |                                        v
   +--> (Many) GENRES                   USER_INTERACTIONS
   +--> (Many) COLLABORATIONS           PLAYLIST_TRACKS
```

---

## 🎵 **1. ARTISTS TABLE**

| Column | Type | Description |
|--------|------|-------------|
| artist_id | UUID | Unique identifier for artist |
| name | VARCHAR(255) | Artist stage name |
| real_name | VARCHAR(255) | Artist real name (optional) |
| biography | TEXT | Artist background story |
| country | VARCHAR(50) | Country of origin |
| birth_date | DATE | Artist birth date |
| genres | JSON | Array of genre IDs |
| social_links | JSON | Social media URLs |
| popularity_score | INT | Global popularity (0-100) |
| verified | BOOLEAN | Is verified artist |
| created_at | TIMESTAMP | Record creation time |

### **JSON Example:**
```json
{
  "artist_id": "art_12345",
  "name": "Lily Allen",
  "real_name": "Lily Rose Beatrice Allen",
  "biography": "British singer-songwriter known for pop and ska influences",
  "country": "United Kingdom",
  "birth_date": "1985-05-02",
  "genres": ["pop", "indie_pop", "ska_pop"],
  "social_links": {
    "spotify": "https://open.spotify.com/artist/13saZpZnCDWOI9D4IJhp1f",
    "instagram": "@lilyallen",
    "twitter": "@lilyallen"
  },
  "popularity_score": 75,
  "verified": true,
  "monthly_listeners": 2500000,
  "follower_count": 890000,
  "created_at": "2023-01-15T10:30:00Z"
}
```

---

## 💿 **2. ALBUMS TABLE**

| Column | Type | Description |
|--------|------|-------------|
| album_id | UUID | Unique album identifier |
| artist_id | UUID | Foreign key to artist |
| title | VARCHAR(255) | Album title |
| album_type | ENUM | album, single, EP, compilation |
| release_date | DATE | Official release date |
| total_tracks | INT | Number of tracks in album |
| genres | JSON | Array of genres |
| record_label | VARCHAR(100) | Publishing record label |
| cover_images | JSON | Album artwork URLs |
| production_info | JSON | Producers, engineers, studios |
| popularity_score | INT | Album popularity (0-100) |

### **JSON Example:**
```json
{
  "album_id": "alb_67890",
  "artist_id": "art_12345",
  "title": "It's Not Me, It's You",
  "album_type": "album",
  "release_date": "2009-02-06",
  "total_tracks": 12,
  "genres": ["pop", "indie_pop", "electropop"],
  "record_label": "Regal Recordings",
  "cover_images": {
    "large": "https://i.scdn.co/image/ab67616d0000b273321bad65a3be5bbd317eb11b",
    "medium": "https://i.scdn.co/image/ab67616d00001e02321bad65a3be5bbd317eb11b",
    "small": "https://i.scdn.co/image/ab67616d00004851321bad65a3be5bbd317eb11b"
  },
  "production_info": {
    "producers": ["Greg Kurstin", "Mark Ronson"],
    "recording_studios": ["Abbey Road Studios"],
    "mixing_engineer": "Tom Elmhirst"
  },
  "popularity_score": 68,
  "chart_positions": {
    "UK": 1,
    "US": 5,
    "Germany": 3
  }
}
```

---

## 🎵 **3. TRACKS TABLE** (Main Music Records)

| Column | Type | Description |
|--------|------|-------------|
| track_id | UUID | Unique track identifier |
| album_id | UUID | Foreign key to album |
| artist_id | UUID | Primary artist |
| title | VARCHAR(255) | Song title |
| duration_ms | INT | Track length in milliseconds |
| track_number | INT | Position in album |
| disc_number | INT | Disc number (for multi-disc albums) |
| explicit | BOOLEAN | Contains explicit content |
| isrc | VARCHAR(20) | International Standard Recording Code |
| audio_features | JSON | Spotify-style audio characteristics |
| lyrics | TEXT | Song lyrics |
| language | VARCHAR(10) | Primary language of lyrics |
| release_date | DATE | Individual track release date |
| popularity_score | INT | Track popularity (0-100) |
| preview_url | VARCHAR(500) | 30-second preview link |
| full_audio_url | VARCHAR(500) | Full audio file URL |
| file_format | VARCHAR(10) | mp3, flac, wav, etc. |
| audio_quality | JSON | Bitrate, sample rate info |

### **JSON Example:**
```json
{
  "track_id": "trk_11111",
  "album_id": "alb_67890",
  "artist_id": "art_12345",
  "title": "Fuck You",
  "duration_ms": 219893,
  "track_number": 8,
  "disc_number": 1,
  "explicit": true,
  "isrc": "GBAYE0802265",
  "audio_features": {
    "acousticness": 0.123,
    "danceability": 0.735,
    "energy": 0.859,
    "instrumentalness": 0.000234,
    "key": 7,
    "liveness": 0.0897,
    "loudness": -4.564,
    "mode": 1,
    "speechiness": 0.0461,
    "tempo": 122.036,
    "time_signature": 4,
    "valence": 0.624
  },
  "lyrics": "Look inside, look inside your tiny mind...",
  "language": "en",
  "release_date": "2009-02-06",
  "popularity_score": 64,
  "preview_url": "https://preview.spotify.com/track/5st5644IlBmKiiRE73UsoZ",
  "full_audio_url": "https://audio.jamendo.com/download/track/123456",
  "file_format": "mp3",
  "audio_quality": {
    "bitrate": "320kbps",
    "sample_rate": "44.1kHz",
    "channels": "stereo"
  },
  "mood_tags": ["angry", "empowering", "sarcastic"],
  "activity_tags": ["breakup", "confidence", "party"],
  "instruments": ["piano", "drums", "bass", "vocals", "synthesizer"]
}
```

---

## 🎼 **4. TRACK_SECTIONS TABLE** (Your Innovation!)

| Column | Type | Description |
|--------|------|-------------|
| section_id | UUID | Unique section identifier |
| track_id | UUID | Foreign key to parent track |
| section_type | ENUM | intro, verse, chorus, bridge, outro, instrumental |
| start_time_ms | INT | Section start time in milliseconds |
| end_time_ms | INT | Section end time in milliseconds |
| duration_ms | INT | Section duration |
| section_order | INT | Order within the track (1, 2, 3...) |
| audio_features | JSON | Audio features specific to this section |
| lyric_content | TEXT | Lyrics for this section |
| section_popularity | FLOAT | How popular this section is (user replays) |
| engagement_metrics | JSON | Skip rate, replay rate, etc. |

### **JSON Example:**
```json
{
  "section_id": "sec_22222",
  "track_id": "trk_11111",
  "section_type": "chorus",
  "start_time_ms": 65000,
  "end_time_ms": 95000,
  "duration_ms": 30000,
  "section_order": 3,
  "audio_features": {
    "energy": 0.95,
    "danceability": 0.85,
    "valence": 0.7,
    "loudness": -3.2,
    "tempo": 122.0,
    "pitch_class_profile": [0.1, 0.05, 0.8, 0.2, 0.1, 0.05, 0.9, 0.15, 0.1, 0.05, 0.2, 0.1],
    "timbre_features": [42.3, -12.1, 8.7, -3.2, 15.6, -7.8, 2.1, -9.4, 11.2, -4.5, 6.3, -2.8]
  },
  "lyric_content": "Fuck you, ooh, ooh, ooh / And fuck her too / And fuck him too / And fuck everybody",
  "section_tags": ["catchy", "memorable", "sing_along"],
  "section_popularity": 0.92,
  "engagement_metrics": {
    "replay_rate": 0.35,
    "skip_rate": 0.08,
    "sing_along_rate": 0.78,
    "emotional_peak": true,
    "climax_section": true
  },
  "musical_elements": {
    "chord_progression": ["Dm", "F", "C", "G"],
    "vocal_range": "A3-D5",
    "backing_vocals": true,
    "instrumental_solo": false
  }
}
```

---

## 👥 **5. COLLABORATIONS TABLE**

| Column | Type | Description |
|--------|------|-------------|
| collaboration_id | UUID | Unique collaboration ID |
| track_id | UUID | Foreign key to track |
| artist_id | UUID | Collaborating artist |
| role | ENUM | featured, producer, songwriter, composer |
| credit_order | INT | Order of credit (1 = main, 2 = secondary) |

### **JSON Example:**
```json
{
  "collaboration_id": "col_33333",
  "track_id": "trk_11111",
  "artist_id": "art_99999",
  "role": "producer",
  "credit_order": 1,
  "contribution_details": {
    "instruments_played": ["piano", "synthesizer"],
    "production_role": "beat_making",
    "percentage_royalty": 15.0
  }
}
```

---

## 🎭 **6. GENRES TABLE**

| Column | Type | Description |
|--------|------|-------------|
| genre_id | UUID | Unique genre identifier |
| name | VARCHAR(50) | Genre name |
| parent_genre_id | UUID | Parent genre (for sub-genres) |
| description | TEXT | Genre description |
| characteristics | JSON | Typical audio features |

### **JSON Example:**
```json
{
  "genre_id": "gen_44444",
  "name": "Indie Pop",
  "parent_genre_id": "gen_11111",
  "description": "Independent pop music with alternative influences",
  "characteristics": {
    "typical_tempo_range": [80, 140],
    "typical_energy": [0.4, 0.8],
    "typical_valence": [0.3, 0.9],
    "common_instruments": ["guitar", "bass", "drums", "keyboards"],
    "production_style": "lo-fi_to_polished"
  },
  "related_genres": ["alternative_rock", "pop", "dream_pop"],
  "popularity_score": 78
}
```

---

## 📱 **7. USER_INTERACTIONS TABLE** (For Recommendations)

| Column | Type | Description |
|--------|------|-------------|
| interaction_id | UUID | Unique interaction ID |
| user_id | UUID | User who interacted |
| track_id | UUID | Track interacted with |
| section_id | UUID | Specific section (optional) |
| interaction_type | ENUM | play, skip, like, save, share, replay |
| timestamp | TIMESTAMP | When interaction occurred |
| context | JSON | Context of interaction |
| device_info | JSON | Device and app info |
| listening_duration_ms | INT | How long user listened |

### **JSON Example:**
```json
{
  "interaction_id": "int_55555",
  "user_id": "usr_77777",
  "track_id": "trk_11111",
  "section_id": "sec_22222",
  "interaction_type": "replay",
  "timestamp": "2024-12-29T14:30:45Z",
  "context": {
    "activity": "workout",
    "location": "gym",
    "time_of_day": "afternoon",
    "weather": "sunny",
    "mood": "energetic",
    "playlist_context": "My Gym Hits",
    "previous_track": "trk_88888"
  },
  "device_info": {
    "device_type": "mobile",
    "os": "iOS",
    "app_version": "1.2.3",
    "audio_quality": "high"
  },
  "listening_duration_ms": 28500,
  "engagement_score": 0.95
}
```

---

## 🎵 **8. PLAYLISTS TABLE**

| Column | Type | Description |
|--------|------|-------------|
| playlist_id | UUID | Unique playlist identifier |
| user_id | UUID | Playlist creator |
| title | VARCHAR(255) | Playlist name |
| description | TEXT | Playlist description |
| playlist_type | ENUM | user_created, algorithmic, editorial |
| activity_context | JSON | Study, gym, party, etc. |
| mood_profile | JSON | Overall mood characteristics |
| target_duration_ms | INT | Intended playlist length |
| is_public | BOOLEAN | Publicly visible |
| created_at | TIMESTAMP | Creation time |

### **JSON Example:**
```json
{
  "playlist_id": "pls_66666",
  "user_id": "usr_77777",
  "title": "Deep Focus Study Session",
  "description": "Instrumental and low-vocal tracks perfect for studying",
  "playlist_type": "user_created",
  "activity_context": {
    "primary_activity": "studying",
    "secondary_activities": ["reading", "writing", "coding"],
    "environment": "quiet_space",
    "duration_target": "2-4_hours"
  },
  "mood_profile": {
    "energy_level": "medium_low",
    "emotional_tone": "calm_focused",
    "intensity": "consistent",
    "progression": "steady"
  },
  "audio_characteristics": {
    "max_speechiness": 0.2,
    "min_instrumentalness": 0.6,
    "tempo_range": [60, 100],
    "energy_range": [0.2, 0.6]
  },
  "target_duration_ms": 7200000,
  "track_count": 45,
  "is_public": true,
  "tags": ["study", "focus", "instrumental", "calm"],
  "created_at": "2024-12-01T09:15:30Z"
}
```

---

## 🔗 **COMPLETE TRACK WITH SECTIONS EXAMPLE**

```json
{
  "track": {
    "track_id": "trk_11111",
    "title": "Fuck You",
    "artist": {
      "artist_id": "art_12345",
      "name": "Lily Allen"
    },
    "album": {
      "album_id": "alb_67890",
      "title": "It's Not Me, It's You"
    },
    "duration_ms": 219893,
    "audio_features": { /* as shown above */ },
    "sections": [
      {
        "section_id": "sec_11111",
        "section_type": "intro",
        "start_time_ms": 0,
        "end_time_ms": 15000,
        "audio_features": { /* intro-specific features */ }
      },
      {
        "section_id": "sec_22222",
        "section_type": "verse",
        "start_time_ms": 15000,
        "end_time_ms": 45000,
        "audio_features": { /* verse-specific features */ }
      },
      {
        "section_id": "sec_33333",
        "section_type": "chorus",
        "start_time_ms": 45000,
        "end_time_ms": 75000,
        "audio_features": { /* chorus-specific features */ },
        "engagement_metrics": {
          "replay_rate": 0.45,
          "skip_rate": 0.05
        }
      }
      // ... more sections
    ]
  }
}
```

This structure gives you everything needed for sophisticated recommendations including section-level analysis!

---

# Data Example
```json
{
  "track_lyrics_enhanced": {
    "track_id": "trk_11111",
    "lyrics_metadata": {
      "has_lyrics": true,
      "lyrics_language": "en",
      "lyrics_source": "official", // official, user_contributed, transcribed
      "lyrics_confidence": 0.95, // AI transcription confidence
      "explicit_content": true,
      "content_warnings": ["profanity", "sexual_content"],
      "lyric_themes": ["breakup", "empowerment", "anger", "relationships"],
      "emotional_sentiment": {
        "overall_sentiment": "negative_to_positive",
        "anger_level": 0.8,
        "sadness_level": 0.3,
        "happiness_level": 0.6,
        "energy_level": 0.9
      },
      "linguistic_features": {
        "word_count": 156,
        "unique_words": 89,
        "reading_level": "intermediate",
        "rhyme_scheme": "ABAB",
        "repetition_ratio": 0.25,
        "chorus_hook_strength": 0.9
      },
      "searchable_content": {
        "full_lyrics_clean": "Look inside look inside your tiny mind then look a bit harder...",
        "full_lyrics_raw": "Look inside, look inside your tiny mind / Then look a bit harder...",
        "lyrics_phonetic": "lʊk ɪnˈsaɪd lʊk ɪnˈsaɪd jʊr ˈtaɪni maɪnd...", // for fuzzy search
        "key_phrases": [
          "look inside your tiny mind",
          "it's not me it's you",
          "and fuck her too"
        ],
        "memorable_lines": [
          "Look inside, look inside your tiny mind",
          "Fuck you, ooh, ooh, ooh"
        ]
      }
    },
    
    "timed_lyrics": [
      {
        "line_id": "line_001",
        "start_time_ms": 15240,
        "end_time_ms": 18960,
        "text": "Look inside, look inside your tiny mind",
        "section_type": "verse",
        "confidence": 0.98,
        "singer": "main_vocal",
        "vocal_style": "conversational"
      },
      {
        "line_id": "line_002", 
        "start_time_ms": 18960,
        "end_time_ms": 22680,
        "text": "Then look a bit harder",
        "section_type": "verse",
        "confidence": 0.97,
        "singer": "main_vocal",
        "vocal_style": "sarcastic"
      },
      {
        "line_id": "line_chorus_01",
        "start_time_ms": 65000,
        "end_time_ms": 68500,
        "text": "Fuck you, ooh, ooh, ooh",
        "section_type": "chorus",
        "confidence": 1.0,
        "singer": "main_vocal",
        "vocal_style": "anthemic",
        "backing_vocals": true,
        "vocal_emphasis": "high"
      }
    ],

    "lyric_sections_detailed": [
      {
        "section_id": "sec_verse_1",
        "section_type": "verse", 
        "start_time_ms": 15000,
        "end_time_ms": 45000,
        "lyrics": "Look inside, look inside your tiny mind / Then look a bit harder...",
        "narrative_function": "setup",
        "emotional_arc": "building_tension",
        "rhyme_pattern": "ABAB",
        "key_words": ["mind", "harder", "find", "smarter"],
        "vocal_delivery": {
          "style": "conversational",
          "pitch_range": "mid",
          "intensity": "medium"
        }
      },
      {
        "section_id": "sec_chorus_1",
        "section_type": "chorus",
        "start_time_ms": 45000,
        "end_time_ms": 75000,
        "lyrics": "Fuck you, ooh, ooh, ooh / And fuck her too...",
        "narrative_function": "emotional_release",
        "emotional_arc": "cathartic_peak",
        "sing_along_factor": 0.95,
        "memorability_score": 0.92,
        "vocal_delivery": {
          "style": "anthemic",
          "pitch_range": "high",
          "intensity": "maximum",
          "backing_vocals": true
        }
      }
    ]
  },

  "lyrics_search_features": {
    "search_indexes": {
      "full_text_search": {
        "indexed_fields": [
          "lyrics_metadata.searchable_content.full_lyrics_clean",
          "lyrics_metadata.searchable_content.key_phrases",
          "timed_lyrics[].text"
        ],
        "search_capabilities": [
          "exact_phrase_match",
          "fuzzy_word_match", 
          "phonetic_search",
          "semantic_similarity"
        ]
      },
      "thematic_search": {
        "indexed_fields": [
          "lyrics_metadata.lyric_themes",
          "lyrics_metadata.emotional_sentiment",
          "lyric_sections_detailed[].narrative_function"
        ]
      },
      "linguistic_search": {
        "indexed_fields": [
          "lyrics_metadata.linguistic_features.rhyme_scheme",
          "lyrics_metadata.linguistic_features.reading_level",
          "lyric_sections_detailed[].rhyme_pattern"
        ]
      }
    },

    "search_use_cases": {
      "lyric_fragment_search": {
        "query": "look inside your mind",
        "matches": ["Look inside, look inside your tiny mind"],
        "fuzzy_tolerance": 0.8
      },
      "emotional_search": {
        "query": "empowering breakup songs",
        "matches_on": ["lyric_themes", "emotional_sentiment.anger_level", "emotional_sentiment.happiness_level"]
      },
      "phonetic_search": {
        "query": "f*ck you",
        "matches_censored": true,
        "phonetic_match": "fʌk ju"
      },
      "mood_based_search": {
        "query": "cathartic chorus",
        "matches_on": ["lyric_sections_detailed[].emotional_arc", "lyric_sections_detailed[].sing_along_factor"]
      }
    }
  },

  "content_moderation": {
    "explicit_content_flags": {
      "profanity_level": "high",
      "sexual_content": "mild",
      "violence": "none",
      "substance_references": "none",
      "content_rating": "explicit"
    },
    "safe_versions": {
      "radio_edit_available": true,
      "clean_version_lyrics": "F*** you, ooh, ooh, ooh",
      "family_friendly_alternative": null
    }
  },

  "multilingual_support": {
    "original_language": "en",
    "translations_available": ["es", "fr", "de", "pt"],
    "translated_versions": {
      "es": {
        "title": "Jódete",
        "lyrics_sample": "Mira dentro, mira dentro de tu mente pequeña...",
        "translation_quality": "professional",
        "cultural_adaptation": true
      }
    }
  }
}
```

---
# Data pipline Architect
## Exctracting data architect
![alt text](image.png)