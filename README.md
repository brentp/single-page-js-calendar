# Single-Page Calendar

A printable single-page year calendar with a JavaScript API for customizing cells.

## Usage

Open `calendar.html` in a browser. By default it shows 2026.

To show a different year, use the URL parameter:
```
calendar.html?year=2027
```

For printing, use landscape orientation and disable headers/footers. The calendar scales to fill any paper size.

## JavaScript API

All methods are available on the global `calendar` object.

### Setting Colors

```javascript
// Set background color (month: 1-12, day: 1-31)
calendar.setColor(1, 15, '#ffcc00');

// Set text color
calendar.setTextColor(1, 15, '#ff0000');
```

### Setting Labels

```javascript
// Add a label to a date
calendar.setLabel(3, 17, 'Holiday');
```

### Combined Updates

```javascript
// Set multiple properties at once
calendar.setCell(6, 15, {
  color: '#e0ffe0',
  textColor: '#006600',
  label: 'Event',
  fontFamily: 'Arial, sans-serif',
  fontSize: '1.2vmin',
  fontWeight: 'bold',
  fontStyle: 'italic'
});
```

### Font Styling

```javascript
// Available font options
calendar.setCell(1, 1, { fontFamily: 'Georgia, serif' });
calendar.setCell(1, 2, { fontSize: '1.5vmin' });
calendar.setCell(1, 3, { fontWeight: 'bold' });    // or '400', '700', etc.
calendar.setCell(1, 4, { fontStyle: 'italic' });   // or 'normal'
```

### Clearing

```javascript
// Clear a single cell
calendar.clearCell(1, 15);

// Clear all customizations
calendar.clearAll();
```

### Date Ranges

```javascript
// Highlight a range of dates
calendar.highlightRange(6, 1, 6, 14, {
  color: '#ffe0e0',
  label: 'Vacation'
});
```

### Finding Dates

```javascript
// Get all Sundays
calendar.getSundays();
// Returns: [{ month: 1, day: 4 }, { month: 1, day: 11 }, ...]

// Get all dates for a specific day of week (0=Sun, 1=Mon, ..., 6=Sat)
calendar.getDayOfWeekDates(5); // All Fridays
```

### Examples

```javascript
// Highlight all Sundays in red
calendar.getSundays().forEach(d => {
  calendar.setColor(d.month, d.day, '#ffd0d0');
});

// Highlight all Fridays in green
calendar.getDayOfWeekDates(5).forEach(d => {
  calendar.setColor(d.month, d.day, '#d0ffd0');
});

// Mark a birthday
calendar.setCell(7, 20, {
  color: '#fffacd',
  label: 'Birthday'
});
```

### Persistence

```javascript
// Export all customizations as JSON
const data = calendar.exportData();
localStorage.setItem('myCalendar', data);

// Import previously saved data
const saved = localStorage.getItem('myCalendar');
if (saved) calendar.importData(saved);
```

### Year Control

```javascript
// Render a different year
calendar.render(2027);

// Get current year
calendar.getYear(); // 2027
```
