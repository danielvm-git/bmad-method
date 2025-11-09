# Date Format Standard - ISO 8601

**Standard**: ISO 8601 with Timezone Offset  
**Version**: 1.0  
**Date**: 2025-11-09T18:00:00-03:00

---

## Overview

All dates and timestamps in BMAD follow **ISO 8601** format with timezone information. This ensures consistency, sortability, and universal parseability across all tools and platforms.

---

## Format Specification

### Standard Format

```
YYYY-MM-DDTHH:MM:SS±HH:MM
```

**Components**:
- `YYYY` - 4-digit year
- `MM` - 2-digit month (01-12)
- `DD` - 2-digit day (01-31)
- `T` - Literal separator between date and time
- `HH` - 2-digit hour (00-23)
- `MM` - 2-digit minute (00-59)
- `SS` - 2-digit second (00-59)
- `±HH:MM` - Timezone offset from UTC

### Examples

**Local Times with Offset**:
```
2025-11-09T14:30:45-03:00  # São Paulo, Brazil (UTC-3)
2025-11-09T09:30:45-05:00  # New York, USA (UTC-5)
2025-11-09T14:30:45+00:00  # London, UK (UTC+0)
2025-11-09T23:00:45+05:30  # Mumbai, India (UTC+5:30)
2025-11-09T22:30:45+09:00  # Tokyo, Japan (UTC+9)
```

**UTC Format** (Alternative notation):
```
2025-11-09T17:30:45Z       # Z is shorthand for +00:00 (UTC)
```

---

## When to Use Each Format

### UTC (Z suffix)

**Use for**:
- Server-generated timestamps
- System logs
- Database records
- Git commit timestamps
- CI/CD pipeline events
- API responses

**Example**:
```yaml
created: "2025-11-09T17:30:45Z"
last_modified: "2025-11-09T18:15:30Z"
```

### Local Time with Offset (±HH:MM)

**Use for**:
- User-facing timestamps
- Scheduled events
- Sprint planning dates
- Meeting times
- Retrospective reports
- Story start/finish times
- Epic deadlines

**Example**:
```yaml
sprint_start: "2025-11-09T09:00:00-03:00"
sprint_end: "2025-11-23T18:00:00-03:00"
meeting_time: "2025-11-10T14:00:00-03:00"
```

---

## Implementation

### JavaScript

#### UTC Format
```javascript
// Generate current time in UTC
function getCurrentUTC() {
  return new Date().toISOString();
}
// Returns: "2025-11-09T17:30:45.000Z"
```

#### Local Time with Offset
```javascript
/**
 * Format date to ISO 8601 with local timezone offset
 * @param {Date} date - Date object to format
 * @returns {string} ISO 8601 format: YYYY-MM-DDTHH:MM:SS±HH:MM
 */
function toISO8601Local(date) {
  // Calculate timezone offset
  const offset = -date.getTimezoneOffset();
  const sign = offset >= 0 ? '+' : '-';
  const hours = String(Math.floor(Math.abs(offset) / 60)).padStart(2, '0');
  const minutes = String(Math.abs(offset) % 60).padStart(2, '0');
  
  // Format date components
  const year = date.getFullYear();
  const month = String(date.getMonth() + 1).padStart(2, '0');
  const day = String(date.getDate()).padStart(2, '0');
  const hour = String(date.getHours()).padStart(2, '0');
  const min = String(date.getMinutes()).padStart(2, '0');
  const sec = String(date.getSeconds()).padStart(2, '0');
  
  return `${year}-${month}-${day}T${hour}:${min}:${sec}${sign}${hours}:${minutes}`;
}

// Usage
const now = new Date();
console.log(toISO8601Local(now));
// Returns: "2025-11-09T14:30:45-03:00" (if in São Paulo)
```

#### Parse ISO 8601
```javascript
// Parse ISO 8601 string to Date object
function parseISO8601(dateString) {
  return new Date(dateString);
}

// Works with both UTC and offset formats
parseISO8601("2025-11-09T17:30:45Z");
parseISO8601("2025-11-09T14:30:45-03:00");
```

#### Calculate Duration
```javascript
/**
 * Calculate duration between two ISO 8601 timestamps
 * @param {string} start - ISO 8601 start time
 * @param {string} finish - ISO 8601 finish time
 * @returns {string} Human-readable duration
 */
function calculateDuration(start, finish) {
  const startDate = new Date(start);
  const finishDate = new Date(finish);
  const diffMs = finishDate - startDate;
  
  const hours = Math.floor(diffMs / (1000 * 60 * 60));
  const minutes = Math.floor((diffMs % (1000 * 60 * 60)) / (1000 * 60));
  const seconds = Math.floor((diffMs % (1000 * 60)) / 1000);
  
  if (hours > 0) {
    return `${hours} hour${hours > 1 ? 's' : ''} ${minutes} minute${minutes > 1 ? 's' : ''}`;
  }
  if (minutes > 0) {
    return `${minutes} minute${minutes > 1 ? 's' : ''} ${seconds} second${seconds > 1 ? 's' : ''}`;
  }
  return `${seconds} second${seconds > 1 ? 's' : ''}`;
}

// Usage
const duration = calculateDuration(
  "2025-11-09T09:00:00-03:00",
  "2025-11-09T17:30:00-03:00"
);
console.log(duration);
// Returns: "8 hours 30 minutes"
```

### YAML

```yaml
# Project timeline with ISO 8601 dates
project:
  name: "My Project"
  created: "2025-11-09T10:00:00-03:00"
  
timeline:
  analysis_start: "2025-11-09T09:00:00-03:00"
  analysis_finish: "2025-11-12T18:00:00-03:00"
  planning_start: "2025-11-13T09:00:00-03:00"
  planning_finish: "2025-11-20T18:00:00-03:00"
  
sprint:
  number: 1
  start_date: "2025-11-21T09:00:00-03:00"
  end_date: "2025-12-04T18:00:00-03:00"
```

### Python

```python
from datetime import datetime, timezone
import pytz

# Get current time in UTC
def get_current_utc():
    return datetime.now(timezone.utc).isoformat()
# Returns: "2025-11-09T17:30:45+00:00"

# Get current time with local timezone
def get_current_local(tz_name="America/Sao_Paulo"):
    tz = pytz.timezone(tz_name)
    return datetime.now(tz).isoformat()
# Returns: "2025-11-09T14:30:45-03:00"

# Parse ISO 8601
def parse_iso8601(date_string):
    return datetime.fromisoformat(date_string)
```

---

## Benefits of ISO 8601

### ✅ Universal Standard
- Globally recognized (RFC 3339, ECMA-262)
- Industry standard for date/time representation
- No regional ambiguity (MM/DD vs DD/MM)

### ✅ Sortable
- Lexicographic sorting works perfectly
- No need for custom sort functions
```bash
# These sort correctly as strings
2025-11-09T09:00:00-03:00
2025-11-09T14:30:45-03:00
2025-11-09T18:45:00-03:00
```

### ✅ Parseable
- Native support in all major languages:
  - JavaScript: `new Date("2025-11-09T14:30:45-03:00")`
  - Python: `datetime.fromisoformat("2025-11-09T14:30:45-03:00")`
  - Go: `time.Parse(time.RFC3339, "2025-11-09T14:30:45-03:00")`
  - Java: `Instant.parse("2025-11-09T17:30:45Z")`

### ✅ Timezone Aware
- Preserves timezone information
- Accurate for distributed teams
- No "what timezone is this?" confusion

### ✅ Tool Compatible
- **Git**: Uses ISO 8601 internally
- **JSON**: `JSON.stringify(date)` produces ISO 8601
- **YAML**: Natively parseable
- **Databases**: PostgreSQL `TIMESTAMPTZ`, MySQL `DATETIME`
- **APIs**: RESTful APIs standard

---

## Special Cases

### Timezones with Half-Hour Offsets

Some timezones use non-integer hour offsets:

```
+05:30  # India Standard Time (IST)
+09:30  # Australian Central Standard Time
-03:30  # Newfoundland Time
+05:45  # Nepal Time
```

**Example**:
```yaml
meeting_india: "2025-11-09T14:00:00+05:30"
meeting_nepal: "2025-11-09T15:15:00+05:45"
```

### Daylight Saving Time (DST)

Timezone offsets automatically adjust for DST:

```yaml
# New York in winter (EST, UTC-5)
winter_meeting: "2025-01-15T10:00:00-05:00"

# New York in summer (EDT, UTC-4)
summer_meeting: "2025-07-15T10:00:00-04:00"
```

**Note**: Offset changes automatically when system timezone observes DST.

### Comparing Times Across Timezones

ISO 8601 makes comparison trivial:

```javascript
// These represent the SAME moment in time
const sao_paulo = "2025-11-09T14:30:45-03:00";
const utc        = "2025-11-09T17:30:45Z";
const tokyo      = "2025-11-10T02:30:45+09:00";

// Comparison works
new Date(sao_paulo) === new Date(utc);      // true
new Date(sao_paulo) === new Date(tokyo);    // true
```

---

## Migration from Old Formats

### Common Legacy Formats

If migrating from legacy formats:

| Old Format | Example | ISO 8601 Equivalent |
|------------|---------|---------------------|
| `MM/DD/YYYY HH:MM:SS` | `11/09/2025 14:30:45` | `2025-11-09T14:30:45-03:00` |
| `DD/MM/YYYY HH:MM:SS` | `09/11/2025 14:30:45` | `2025-11-09T14:30:45-03:00` |
| `YYYY/MM/DD HH:MM:SS` | `2025/11/09 14:30:45` | `2025-11-09T14:30:45-03:00` |
| Unix timestamp | `1731175845` | `2025-11-09T17:30:45Z` |

### Conversion Script (JavaScript)

```javascript
/**
 * Convert legacy date formats to ISO 8601
 * @param {string} legacyDate - Date in legacy format
 * @param {string} format - Legacy format type
 * @returns {string} ISO 8601 formatted date
 */
function convertToISO8601(legacyDate, format = 'MM/DD/YYYY HH:MM:SS') {
  // Parse according to format
  let date;
  
  if (format === 'MM/DD/YYYY HH:MM:SS') {
    const [datePart, timePart] = legacyDate.split(' ');
    const [month, day, year] = datePart.split('/');
    date = new Date(`${year}-${month}-${day}T${timePart}`);
  }
  // Add other formats as needed...
  
  // Convert to ISO 8601 local
  return toISO8601Local(date);
}
```

---

## Validation

### Valid ISO 8601 Formats

✅ Correct:
```
2025-11-09T14:30:45-03:00
2025-11-09T17:30:45Z
2025-11-09T14:30:45+00:00
2025-11-09T23:00:45+05:30
```

❌ Invalid:
```
2025-11-09 14:30:45        # Missing T separator
2025/11/09T14:30:45        # Wrong date separator
2025-11-09T14:30:45-3      # Missing minutes in offset
11/09/2025T14:30:45        # Wrong date order
2025-11-09T14:30:45 PST    # Text timezone (not offset)
```

### Regex Validation

```javascript
const iso8601Regex = /^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}([+-]\d{2}:\d{2}|Z)$/;

function isValidISO8601(dateString) {
  return iso8601Regex.test(dateString);
}

// Usage
isValidISO8601("2025-11-09T14:30:45-03:00");  // true
isValidISO8601("2025-11-09T17:30:45Z");        // true
isValidISO8601("2025/11/09 14:30:45");         // false
```

---

## Best Practices

### DO ✅

1. **Always include timezone information**
   ```yaml
   # Good
   created: "2025-11-09T14:30:45-03:00"
   ```

2. **Use UTC for server/system timestamps**
   ```yaml
   # Good for logs
   log_timestamp: "2025-11-09T17:30:45Z"
   ```

3. **Use local time for user-facing dates**
   ```yaml
   # Good for meetings
   meeting_time: "2025-11-10T10:00:00-03:00"
   ```

4. **Store in UTC, display in local**
   ```javascript
   // Store
   database.save({ created: new Date().toISOString() });
   
   // Display
   const local = toISO8601Local(new Date(record.created));
   ```

### DON'T ❌

1. **Don't use ambiguous formats**
   ```yaml
   # Bad - what timezone?
   created: "2025-11-09 14:30:45"
   ```

2. **Don't mix formats**
   ```yaml
   # Bad - inconsistent
   start: "2025-11-09T14:30:45-03:00"
   finish: "11/09/2025 17:30:45"
   ```

3. **Don't truncate timezone**
   ```yaml
   # Bad - not ISO 8601 compliant
   created: "2025-11-09T14:30:45-03"
   ```

---

## References

- [ISO 8601 Standard](https://www.iso.org/iso-8601-date-and-time-format.html)
- [RFC 3339 (Date and Time on the Internet)](https://tools.ietf.org/html/rfc3339)
- [ECMA-262 Date Time String Format](https://tc39.es/ecma262/#sec-date-time-string-format)
- [Wikipedia: ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)

---

**Last Updated**: 2025-11-09T18:00:00-03:00  
**Version**: 1.0  
**Status**: Active Standard

