# Timezone converter

Build a link to open [Savvy Time Zone Converter](https://savvytime.com/converter) with the search already provided.  
The final `Timezone URL builder` snippet will be composed of other re-usable snippets and macros.

```text title="Timezones (snippet)"
{{PST|UTC|CET|CEST|IST}}
```

```js title="Year List (macro)"
const currentYear = new Date().getFullYear();
const years = [
  currentYear - 2,
  currentYear - 1,
  currentYear,
  currentYear + 1,
  currentYear + 2,
];

export default `{{${years.join("|")}}}`;
```

```text title="Month List (snippet)"
{{Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec}}
```

```js title="Day List (macro)"
const daysInMonth = 31;
const days = [];

for (let day = 1; day <= daysInMonth; day++) {
  days.push(day);
}

export default `{{${days.join("|")}}}`;
```

```js title="Hour List (macro)"
const hours = 24;
const timeStrings = [];

for (let hour = 0; hour < hours; hour++) {
  const hourString = hour.toString().padStart(2, "0");
  timeStrings.push(hourString);
}

export default `{{${timeStrings.join("|")}}}`;
```

```js title="Minute List (macro)"
const minutes = 60;
const minuteStrings = [];

for (let minute = 0; minute < minutes; minute++) {
  const minuteString = minute.toString().padStart(2, "0");
  minuteStrings.push(minuteString);
}

export default `{{${minuteStrings.join("|")}}}`;
```

```md title="Timezone URL builder (snippet)"
https://savvytime.com/converter/$Timezones-to-$Timezones/$MonthList-$DayList-$YearList/$HourList-$MinuteList
```
