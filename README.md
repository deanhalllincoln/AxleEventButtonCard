# ⚡ Axle VPP Event Card

A modern, dynamic Home Assistant dashboard card that displays the current status of your **Axle Virtual Power Plant (VPP)** events.

The card automatically appears when an Axle event is scheduled, provides a live countdown to the event, switches to a remaining time display once the event begins, and disappears again when no event is planned.

---

# Features

> **Completed Card**
>
> <img width="270" height="153" alt="Screenshot_20260716_210024_Home Assistant" src="https://github.com/user-attachments/assets/74e9072e-8424-47b1-b012-470d27a8b453" />



## 🚀 Automatic Event Detection (Card will be hidden when no event is planned)

The card automatically displays whenever:

- An Axle VPP event is scheduled
- An Axle VPP event is currently in progress

When no event exists, the card is automatically hidden, keeping your dashboard clean.

---

## ⏳ Live Countdown

Displays the time remaining until the next Axle VPP event begins.

Example:

```
Starts In

2h 15m
```

---

## ⚡ Live Event Timer

Once an event starts, the countdown automatically changes to display the remaining event duration.

Example:

```
Remaining

47m
```

---

## 🎨 Automatic Colour Changes

The card changes colour depending on the current event status.

| Status | Colour |
|---------|--------|
| Upcoming Event | 🔵 Blue |
| Event Active | 🟢 Green |

---

## 💡 One Hour Warning pulsing Card

During the final **60 minutes** before an event begins, the card gently pulses to attract attention between green and blue.

This provides an easy visual reminder that an event is about to start.

<img width="270" height="154" alt="Screenshot_20260716_195605_Home Assistant" src="https://github.com/user-attachments/assets/323c9f3f-f5b2-4315-b4a6-710e29ad471e" /> <img width="270" height="153" alt="Screenshot_20260716_195637_Home Assistant" src="https://github.com/user-attachments/assets/8de22295-3926-4939-a045-4018ff52f7d4" />


---

## 📅 Event Schedule

Displays:

- Start Date
- Start Time
- End Date
- End Time

Example:

```
Start: Tue 15 Jul 2026 18:00

End:   Tue 15 Jul 2026 19:00
```
<img width="270" height="155" alt="Screenshot_20260716_200054_Home Assistant" src="https://github.com/user-attachments/assets/35921f64-c0a7-45d5-849f-1ee1e36cd145" />
Note: When the event is taking place the card says "Remaining"

---

## 📱 Responsive Design

Designed to work perfectly on:

- Desktop
- Tablet
- Mobile
- Home Assistant Companion App

---

# Requirements

Before installing this card you must have:

- Home Assistant
- Axle VPP Integration
- Button Card (via HACS)
- Card Mod (via HACS)

---

# Entities Used

The card uses the following entities created by the Axle VPP Integration.

| Entity | Description |
|----------|-------------|
| `sensor.axle_event_in_progress` | Indicates whether an event is active |
| `sensor.axle_event_minutes_to_start_2` | Countdown until next event |
| `sensor.axle_event_remaining_minutes` | Remaining event duration |
| `sensor.axle_start_time_friendly` | Friendly event start time |
| `sensor.axle_end_time_friendly` | Friendly event end time |
| `sensor.axle_event_window_state_2` | Determines when the card is displayed |

---

# Installation

---

# Step 1 - Install Button Card

> **Install Button Card from HACS**
>
> <img width="1551" height="418" alt="image" src="https://github.com/user-attachments/assets/48cdaac5-eb19-4cb5-a91b-239e3b11cb38" />


1. Open **HACS**
2. Select **Frontend**
3. Click **Explore & Download Repositories**
4. Search for:

```
Button Card
```

5. Install the card.
6. Restart Home Assistant.

---

# Step 2 - Verify the Axle Sensors ( Iassume you have the Axle integration installed)

<img width="1300" height="748" alt="image" src="https://github.com/user-attachments/assets/54c7886a-fd67-4c12-9f57-a9f1754c843f" />


Open:

**Developer Tools → States**

Confirm the following entities exist.

- `sensor.axle_event_in_progress`
- `sensor.axle_event_minutes_to_start_2`
- `sensor.axle_event_remaining_minutes`
- `sensor.axle_start_time_friendly`
- `sensor.axle_end_time_friendly`
- `sensor.axle_event_window_state_2`

If any are missing, ensure the Axle VPP Integration has been installed correctly.

---

# Step 3 - Add the Card to your Dashboard

> **📷 Screenshot Placeholder – Edit Dashboard**
>
> *(Insert screenshot here)*

1. Open your Home Assistant dashboard.
2. Select **Edit Dashboard**.
3. Click **The plus sign where you want to add the card**.
4. Select **Add by Card**.
5. Search for "Button-card"

---

# Step 4 - Paste the YAML

<img width="699" height="571" alt="image" src="https://github.com/user-attachments/assets/0a754fa1-015a-4b11-9923-c5c7c6e1b61a" />


Delete any existing YAML.

Copy and paste the complete card YAML supplied below.

```yaml
type: custom:button-card
entity: sensor.axle_event_in_progress
name: ""
show_name: false
show_icon: false
show_state: false
variables:
  in_progress: |
    [[[ return states['sensor.axle_event_in_progress']?.state === 'on'; ]]]
  mins_to_start: >
    [[[ return parseInt(states['sensor.axle_event_minutes_to_start_2']?.state ||
    0); ]]]
  mins_remaining: >
    [[[ return parseInt(states['sensor.axle_event_remaining_minutes']?.state ||
    0); ]]]
  start_time: |
    [[[ return states['sensor.axle_start_time_friendly']?.state || '-'; ]]]
  end_time: |
    [[[ return states['sensor.axle_end_time_friendly']?.state || '-'; ]]]
custom_fields:
  header: |
    [[[ return `
      <div style="
        position:absolute;
        top:14px;
        left:14px;
        display:flex;
        align-items:center;
        gap:10px;
        font-size:18px;
        font-weight:600;
        z-index:10;
        pointer-events:none;
      ">
        <span style="color:#ffcc00;font-size:20px;line-height:1;">⚡</span>
        <span>Axle Virtual Power Plant</span>
      </div>
    `; ]]]
  timer: |
    [[[ 
      const inProgress = states['sensor.axle_event_in_progress']?.state === 'on';

      const rawStart = states['sensor.axle_event_minutes_to_start_2']?.state;
      const rawRemain = states['sensor.axle_event_remaining_minutes']?.state;

      const minsToStart = parseInt(rawStart);
      const minsRemaining = parseInt(rawRemain);

      const hasValidStart = !isNaN(minsToStart) && rawStart !== null && rawStart !== '' && rawStart !== 'unknown';
      const hasValidRemain = !isNaN(minsRemaining) && rawRemain !== null && rawRemain !== '' && rawRemain !== 'unknown';

      function formatDuration(totalMins) {
        const days = Math.floor(totalMins / 1440);
        const hours = Math.floor((totalMins % 1440) / 60);
        const mins = totalMins % 60;

        let parts = [];

        if (days > 0) {
          parts.push(`${days}d`);
        }

        if (hours > 0 || days > 0) {
          parts.push(`${hours}h`);
        }

        parts.push(`${mins}m`);

        return parts.join(' ');
      }

      if (!inProgress && !hasValidStart) {
        return `
          <div style="width:100%;text-align:center;">
            <div style="font-size:18px;font-weight:500;opacity:0.85;">
              No Event Planned
            </div>
          </div>
        `;
      }

      if (inProgress && hasValidRemain) {
        return `
          <div style="width:100%;text-align:center;">
            <div style="font-size:18px;font-weight:500;opacity:0.85;margin-bottom:6px;">
              Remaining
            </div>
            <div style="font-size:42px;font-weight:700;line-height:1;">
              ${formatDuration(minsRemaining)}
            </div>
          </div>
        `;
      }

      if (!inProgress && hasValidStart) {
        return `
          <div style="width:100%;text-align:center;">
            <div style="font-size:18px;font-weight:500;opacity:0.85;margin-bottom:6px;">
              Starts In
            </div>
            <div style="font-size:42px;font-weight:700;line-height:1;">
              ${formatDuration(minsToStart)}
            </div>
          </div>
        `;
      }

      return `
        <div style="width:100%;text-align:center;font-size:18px;opacity:0.7;">
          Waiting for event data...
        </div>
      `;
    ]]]
  times: |
    [[[
      function formatTime(value) {
        if (!value || value === 'unknown' || value === 'unavailable')
          return '-';

        const d = new Date(value);

        return d.toLocaleString([], {
          weekday: 'short',
          day: 'numeric',
          month: 'short',
          year: 'numeric',
          hour: 'numeric',
          minute: '2-digit'
        });
      }

      const start = formatTime(states['sensor.axle_start_time_friendly']?.state);
      const end = formatTime(states['sensor.axle_end_time_friendly']?.state);

      if (start === '-' && end === '-') return '';

      return `
        <div style="font-size:13px;opacity:0.85;line-height:1.5;">
          <div style="display:flex;">
            <div style="width:55px;">Start:</div>
            <div>${start}</div>
          </div>
          <div style="display:flex;">
            <div style="width:55px;">End:</div>
            <div>${end}</div>
          </div>
        </div>
      `;
    ]]]
styles:
  card:
    - display: |
        [[[
          const upcoming = states['sensor.axle_event_window_state_2']?.state === 'upcoming';
          const active = states['sensor.axle_event_in_progress']?.state === 'on';

          return (upcoming || active) ? 'block' : 'none';
        ]]]
    - padding: 12px
    - border-radius: 16px
    - min-height: 150px
    - color: white
    - box-shadow: 0px 6px 18px rgba(0,0,0,0.35)
    - text-align: left
    - background: |
        [[[ 
          return states['sensor.axle_event_in_progress']?.state === 'on'
            ? 'linear-gradient(135deg, #1b5e20, #0d1f0d)'
            : 'linear-gradient(135deg, #1a237e, #0b0f2a)';
        ]]]
  grid:
    - grid-template-areas: "\"header\" \"timer\" \"times\""
    - grid-template-columns: 1fr
    - justify-items: start
    - align-items: start
  custom_fields:
    header:
      - grid-area: header
      - justify-self: start
      - width: 100%
    timer:
      - grid-area: timer
      - width: 100%
      - justify-self: stretch
      - margin-top: 35px
      - margin-bottom: 12px
    times:
      - grid-area: times
      - justify-self: start
      - margin-top: 10px
      - opacity: 0.85
card_mod:
  style: |
    @keyframes pulse {
      0% {
        background: linear-gradient(135deg, #1a237e, #0b0f2a);
      }
      50% {
        background: linear-gradient(135deg, #1b5e20, #0d1f0d);
      }
      100% {
        background: linear-gradient(135deg, #1a237e, #0b0f2a);
      }
    }

    ha-card {
      {% if is_state('sensor.axle_event_in_progress', 'off')
         and states('sensor.axle_event_minutes_to_start_2')|int < 60 %}
        animation: pulse 1.5s ease-in-out infinite;
        border: 1px solid rgba(255, 200, 0, 0.5);
      {% endif %}
    }

```

Click **Save**.

---

# Step 5 - Finished! ( The card will only show in the dashboard when there is an event planned)

The card will now operate automatically.

### Upcoming Event

- Card will show in your dashboard
- Blue background
- Countdown timer
- Event start and end times

### Less than 60 Minutes Before Start

- Blue background pulsing to green
- Countdown timer

### Event Active

- Green background
- Remaining event time
- Event schedule

### No Event

The card automatically hides itself.

---

# Card Behaviour

| Condition | Behaviour |
|------------|-----------|
| No Event Scheduled | Hidden |
| Event Scheduled | Blue countdown card |
| Less than 60 Minutes | Animated blue countdown |
| Event Active | Green remaining timer |
| Event Finished | Hidden |

---

# Troubleshooting

| Problem | Solution |
|----------|----------|
| Card never appears | Verify `sensor.axle_event_window_state_2` is `upcoming` or `sensor.axle_event_in_progress` is `on`. |
| Countdown missing | Check `sensor.axle_event_minutes_to_start_2` contains a numeric value. |
| Start or end times missing | Verify `sensor.axle_start_time_friendly` and `sensor.axle_end_time_friendly`. |
| Card displays errors | Confirm Button Card is installed and up to date. |

---

# Compatibility

Tested with:

- Home Assistant
- Button Card
- Axle VPP Integration

---

# Support

If you experience any issues or have suggestions for improvements, please open an issue on the GitHub repository.

---

# Version History

| Version | Changes |
|---------|---------|
| **1.0** | Initial release |
