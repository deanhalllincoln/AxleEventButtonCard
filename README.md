# ⚡ Axle VPP Event Card

A modern, dynamic Home Assistant dashboard card that displays the current status of your **Axle Virtual Power Plant (VPP)** events.

The card automatically appears when an Axle event is scheduled, provides a live countdown to the event, switches to a remaining time display once the event begins, and disappears again when no event is planned.

---

# Features

> **Completed Card**

<img width="270" height="153" alt="image" src="https://github.com/user-attachments/assets/c321ff6f-9409-45ac-a7f2-42186013da9e" />



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

<img width="270" height="154" alt="image" src="https://github.com/user-attachments/assets/b54383c6-c2bb-4525-9bf4-60ea07e60ab3" /> <img width="270" height="154" alt="image" src="https://github.com/user-attachments/assets/1a78a94d-ed53-4fbe-bdd1-21aeb3d87158" />


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
<img width="270" height="154" alt="image" src="https://github.com/user-attachments/assets/8d4fc3c6-79fc-407a-89d6-2daeb01e5fa4" />

Note: When the event is taking place the card says "LIVE" and a bar moves across the card indicating time remaining


---

## 📱 Responsive Design

Designed to work perfectly on:

- Desktop
- Tablet
- Mobile
- Home Assistant Companion App

---

# Requirements

## Requirements

Before installing this card you must have:

- Home Assistant (Ideally on the latest or very recent version)
- [Axle VPP Integration](https://github.com/deanhalllincoln/ha-axle-vpp) (via HACS)
- [Button Card](https://github.com/custom-cards/button-card) (via HACS)
- [Card Mod](https://github.com/thomasloven/lovelace-card-mod) (via HACS)

---

# Entities Used

The card uses the following entities created by the Axle VPP Integration.

| Entity | Description |
|----------|-------------|
| `sensor.axle_event_in_progress` | Indicates whether an event is active |
| `sensor.axle_event_minutes_to_start` | Countdown until next event |
| `sensor.axle_event_remaining_minutes` | Remaining event duration |
| `sensor.axle_start_time_friendly` | Friendly event start time |
| `sensor.axle_end_time_friendly` | Friendly event end time |
| `sensor.axle_event_window_state` | Determines when the card is displayed |

---

# Installation

---

# Step 1 - Install Button Card

> **Install Button Card from HACS**
>
> <img width="1582" height="347" alt="image" src="https://github.com/user-attachments/assets/73b14bf1-4824-4122-894c-068d99c0c878" />



1. Open **HACS**
2. Select **Dashboard**
3. Click **Explore & Download Repositories**
4. Search for:

```
Button Card
```

5. Install the card. ( For referance the Github repoository is https://github.com/custom-cards/button-card)
6. Restart Home Assistant.

---

# Step 2 - Install Card-Mod (From Lovelace this allows greater control of styling for HA frontend)

> **Install Card-Mod from HACS**
>
> <img width="1570" height="345" alt="image" src="https://github.com/user-attachments/assets/86ff8f61-199a-4081-8127-5b5216929c97" />


1. Open **HACS**
2. Select **Frontend**
3. Click **Explore & Download Repositories**
4. Search for:

```
Card-Mod
```

5. Install the Card-Mod CSS styling . ( For referance the Github repoository is https://github.com/thomasloven/lovelace-card-mod)
6. Restart Home Assistant.

---


# Step 3 - Verify the Axle Sensors ( Iassume you have the Axle integration installed)

<img width="1300" height="748" alt="image" src="https://github.com/user-attachments/assets/54c7886a-fd67-4c12-9f57-a9f1754c843f" />


Open:

**Developer Tools → States**

Confirm the following entities exist.

- `sensor.axle_event_in_progress`
- `sensor.axle_event_minutes_to_start`
- `sensor.axle_event_remaining_minutes`
- `sensor.axle_start_time_friendly`
- `sensor.axle_end_time_friendly`
- `sensor.axle_event_window_state`

If any are missing, ensure the Axle VPP Integration has been installed correctly.

---

# Step 4 - Add the Card to your Dashboard

> **📷 Screenshot Placeholder – Edit Dashboard**
>
> *(Insert screenshot here)*

1. Open your Home Assistant dashboard.
2. Select **Edit Dashboard**.
3. Click **The plus sign where you want to add the card**.
4. Select **Add by Card**.
5. Search for "Button-card"

---

# Step 5 - Paste the YAML

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
    [[[ return parseInt(states['sensor.axle_event_minutes_to_start']?.state ||
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
        <ha-icon icon="mdi:transmission-tower-export" style="
          color: var(--warning-color);
          width:22px;
          height:22px;
          --mdc-icon-size:22px;
        ">
        </ha-icon>
        <span>Axle Virtual Power Plant</span>
      </div>
    `; ]]]
  timer: |
    [[[ 
      const inProgress = states['sensor.axle_event_in_progress']?.state === 'on';

      const rawStart = states['sensor.axle_event_minutes_to_start']?.state;
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
            <div style="font-size:16px;font-weight:600;opacity:0.85;margin-bottom:6px;letter-spacing:1px;">
              LIVE
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
          <div style="display:flex;flex-wrap:wrap;">
            <div style="flex:0 0 auto;margin-right:6px;">Start:</div>
            <div style="flex:1 1 auto;min-width:0;">${start}</div>
          </div>
          <div style="display:flex;flex-wrap:wrap;">
            <div style="flex:0 0 auto;margin-right:6px;">End:</div>
            <div style="flex:1 1 auto;min-width:0;">${end}</div>
          </div>
        </div>
      `;
    ]]]
  progress: |
    [[[
      const active = states['sensor.axle_event_in_progress']?.state === 'on';

      if (!active) return '';

      const start = states['sensor.axle_start_time_friendly']?.state;
      const end = states['sensor.axle_end_time_friendly']?.state;
      const remainRaw = states['sensor.axle_event_remaining_minutes']?.state;

      let percent = 0;

      if (start && end && start !== 'unknown' && end !== 'unknown') {
        const totalMins = (new Date(end) - new Date(start)) / 60000;
        const remain = parseInt(remainRaw || 0);

        if (totalMins > 0) {
          percent = Math.min(100, Math.max(0, Math.round(((totalMins - remain) / totalMins) * 100)));
        }
      }

      return `
        <div style="
          width:100%;
          height:10px;
          background:rgba(255,255,255,0.15);
          border-radius:10px;
          overflow:hidden;
        ">
          <div style="
            width:${percent}%;
            height:100%;
            background:linear-gradient(90deg,#42a5f5,#66bb6a);
            border-radius:10px;
            transition:width 60s linear;
          "></div>
        </div>
      `;
    ]]]
styles:
  card:
    - display: |
        [[[
          const upcoming = states['sensor.axle_event_window_state']?.state === 'upcoming';
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
    - grid-template-areas: "\"header\" \"timer\" \"times\" \"progress\""
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
    progress:
      - grid-area: progress
      - width: 100%
      - justify-self: stretch
      - margin-top: 12px
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
         and states('sensor.axle_event_minutes_to_start')|int < 60 %}
        animation: pulse 1.5s ease-in-out infinite;
        border: 1px solid rgba(255, 200, 0, 0.5);
      {% endif %}
    }


```

Click **Save**.

---

# Step 6 - Finished! ( The card will only show in the dashboard when there is an event planned)

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
| Card never appears | Verify `sensor.axle_event_window_state` is `upcoming` or `sensor.axle_event_in_progress` is `on`. |
| Countdown missing | Check `sensor.axle_event_minutes_to_start` contains a numeric value. |
| Start or end times missing | Verify `sensor.axle_start_time_friendly` and `sensor.axle_end_time_friendly`. |
| Card displays errors | Confirm Button Card is installed and up to date. |

---

# Compatibility

Tested with:

- Home Assistant
- Button Card
- Axle VPP Integration

---

# Testing the Card

If you don't currently have an Axle VPP event scheduled, you can still confirm that the card has been installed correctly.

The following tests temporarily change the state of the Axle sensors using Home Assistant's **Developer Tools**.

> **Note**
>
> These changes are only temporary. The Axle VPP integration will automatically restore the correct values within a few seconds.
> Its easier if you have two screens open at the same time (one Developer tools and one the dashboard showing the card)

---

## Test 1 - Simulate an Active Event

This test confirms that the card displays correctly during an active Axle VPP event.

### Step 1

Open:

**Developer Tools → States**

### Step 2

Locate the entity:

```
sensor.axle_event_in_progress
```

### Step 3

Change the **State** from:

```
off
```

to

```
on
```

Then click **Set State**.

<img width="485" height="193" alt="image" src="https://github.com/user-attachments/assets/ee3d3d76-7398-49cd-8fac-18750c492106" />



### Expected Result

The card should immediately appear on your dashboard with:

- 🟢 Green background
- "Waiting for event data..." message (or no event information)
- No countdown or event times

This confirms that the card is correctly detecting an active event.

After a few seconds, the Axle VPP integration will automatically change the sensor back to its real value and the card will disappear again.

---

## Test 2 - Simulate an Upcoming Event

This test confirms that the card displays correctly before an event starts.

### Step 1

Open:

**Developer Tools → States**

### Step 2

Locate the entity:

```
sensor.axle_event_window_state
```

### Step 3

Change the **State** to:

```
upcoming
```

Then click **Set State**.

<img width="488" height="196" alt="image" src="https://github.com/user-attachments/assets/c229b22b-6b6f-4f22-8753-e44878344953" />


### Expected Result

The card should immediately appear with:

- 🔵 Blue background
- "No Event Planned" message 
- No countdown or event times

This confirms that the card is correctly detecting an upcoming event.

After a few seconds, the Axle VPP integration will automatically restore the correct state and the card will disappear again.

---

## Successful Test

If both tests behave as described, your Button Card has been installed correctly and is ready to display live Axle VPP events when they are received.

# Support

If you experience any issues or have suggestions for improvements, please open an issue on the GitHub repository.

---

# Version History

| Version | Changes |
|---------|---------|
| **1.0** | Initial release |
