# Single-Page Calendar

A printable single-page year calendar with a JavaScript API for customizing cells. Supports import/export and URL sharing.

Inspired by [neatnik.net/calendar](https://neatnik.net/calendar/?year=2026).

## Usage

Open `calendar.html` in a browser or use the [hosted version](https://brentp.github.io/single-page-js-calendar/calendar.html).

```
calendar.html?year=2027              # Show specific year
calendar.html?data=eJxLzy...         # Load shared calendar data
```

**Printing:** Use landscape orientation, disable headers/footers. Scales to any paper size.

## JavaScript API

Open your browser's developer console (F12 or Ctrl+Shift+j) to use these methods. All methods are on the global `calendar` object.

### Basic Styling

```javascript
calendar.setColor(1, 15, '#ffcc00');      // Background (month: 1-12, day: 1-31)
calendar.setTextColor(1, 15, '#ff0000');  // Text color
calendar.setLabel(3, 17, 'Holiday');      // Add label
```

### Full Cell Control

```javascript
calendar.setCell(6, 15, {
  color: '#e0ffe0',
  textColor: '#006600',
  label: 'Event',
  fontFamily: 'Arial, sans-serif',
  fontSize: '1.2vmin',
  fontWeight: 'bold',              // or '400', '700', etc.
  fontStyle: 'italic'              // or 'normal'
});
```

### Borders

```javascript
// Bottom border (underline)
calendar.setCell(1, 15, { borderBottomColor: '#ff0000', borderBottomWidth: '3px' });

// Left border (mark start of periods)
calendar.setCell(6, 1, { borderLeftColor: '#0066cc', borderLeftWidth: '4px' });

// Combine with ranges
calendar.setRange(6, 1, 6, 14, { borderBottomColor: '#0066cc', borderBottomWidth: '2px' });
```

### Date Ranges

```javascript
calendar.setRange(6, 1, 6, 14, { color: '#ffe0e0', label: 'Vacation' });
```

### Finding Dates

```javascript
calendar.getSundays();           // [{ month: 1, day: 4 }, ...]
calendar.getDayOfWeekDates(5);   // All Fridays (0=Sun, 6=Sat)

// Highlight all Sundays
calendar.getSundays().forEach(d => calendar.setColor(d.month, d.day, '#ffd0d0'));
```

### Clearing

```javascript
calendar.clearCell(1, 15);  // Single cell
calendar.clearAll();        // All customizations
```

### Import/Export

```javascript
// JSON export
const data = calendar.exportData();
calendar.importData(data);

// Compressed base64 for URL sharing
calendar.exportData(true).then(b64 => {
  console.log(`calendar.html?data=${b64}`);
});
```

### Year Control

```javascript
calendar.render(2027);   // Render different year
calendar.getYear();      // Get current year
```
