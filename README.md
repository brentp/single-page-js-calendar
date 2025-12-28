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

### Cell Styling

```javascript
calendar.setCell(6, 15, {
  color: '#e0ffe0',           // Background color
  textColor: '#006600',       // Text color
  label: '<b>Event</b><br>other line',             // Label text (supports HTML)
  fontFamily: 'Arial, sans-serif',
  fontSize: '1.2vmin',
  fontWeight: 'bold',         // or '400', '700', etc.
  fontStyle: 'italic'         // or 'normal'
});

// Labels support HTML
calendar.setCell(12, 25, { label: '<b>Christmas</b>' });
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
calendar.getSundays().forEach(d => calendar.setCell(d.month, d.day, { color: '#ffd0d0' }));
```

### Clearing

```javascript
calendar.clearCell(1, 15);  // Single cell
calendar.clearAll();        // All customizations
```

### Import/Export

```javascript
// JSON string export/import
const jsonString = calendar.exportData();
calendar.importData(jsonString);
```

**NOTE** below can be a nice way to save/share modifiable data.

// Raw JavaScript object (no serialization)
const data = calendar.exportJSONData();
calendar.importJSONData({
    year: 2026, cells: { "2026-01-15": { color: "#ffcc00", label: "Event" }, ... } 
})

// Compressed base64 for URL sharing
calendar.exportData(true).then(b64 => {
  console.log(`calendar.html?data=${b64}`);
});
```

### Sharing

The **Share** button creates a URL with your calendar data encoded in the query string. This works well for calendars with moderate customization, but browsers have URL length limits (typically 2,000-8,000 characters depending on the browser).

For heavily customized calendars that exceed URL limits, save the JavaScript instead:

```javascript
// Copy this output and paste it into the console to restore
console.log(`calendar.importJSONData(${JSON.stringify(calendar.exportJSONData())});`);
```

### Local Storage

The **Save** button persists your calendar to the browser's localStorage. Data is automatically loaded on page refresh. The Save button appears **bold** when there are unsaved changes.

The **Clear** button removes all customizations from the current calendar.

```javascript
// Save/load with default key
calendar.saveToLocalStorage();
calendar.loadFromLocalStorage();

// Save/load with custom name (for multiple calendars)
calendar.saveToLocalStorage('work-calendar');
calendar.loadFromLocalStorage('work-calendar');

// Merge stored data with current calendar (keeps current year, combines cells)
calendar.loadFromLocalStorage('holidays', true);

// Clear stored data
calendar.clearLocalStorage();
calendar.clearLocalStorage('work-calendar');
```

### Year Control

```javascript
calendar.render(2027);   // Render different year
calendar.getYear();      // Get current year
```
