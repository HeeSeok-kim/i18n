---
id: clock
title: Das Clock-Objekt
---

Sie können die Browser-Systemuhr mit dem Befehl [`emulate`](/docs/emulation) modifizieren. Dies überschreibt native globale Funktionen im Zusammenhang mit der Zeit und ermöglicht deren synchrone Steuerung über `clock.tick()` oder das erzeugte Clock-Objekt. Dies umfasst die Kontrolle über:

- `setTimeout`
- `clearTimeout`
- `setInterval`
- `clearInterval`
- `Date Objects`

Die Uhr beginnt bei der Unix-Epoche (Zeitstempel 0). Das bedeutet, wenn Sie in Ihrer Anwendung ein neues Date-Objekt instanziieren, wird es die Zeit des 1. Januar 1970 haben, sofern Sie keine anderen Optionen an den Befehl `emulate` übergeben.

## Beispiel

Wenn Sie `browser.emulate('clock', { ... })` aufrufen, werden sofort die globalen Funktionen für die aktuelle Seite sowie alle folgenden Seiten überschrieben, z.B.:

```ts
const clock = await browser.emulate('clock', { now: new Date(1989, 7, 4) })

console.log(await browser.execute(() => (new Date()).toString()))
// returns "Fri Aug 04 1989 00:00:00 GMT-0700 (Pacific Daylight Time)"

await browser.url('https://webdriverio')
console.log(await browser.execute(() => (new Date()).toString()))
// returns "Fri Aug 04 1989 00:00:00 GMT-0700 (Pacific Daylight Time)"

await clock.restore()

console.log(await browser.execute(() => (new Date()).toString()))
// returns "Thu Aug 01 2024 17:59:59 GMT-0700 (Pacific Daylight Time)"

await browser.url('http://guinea-pig.webdriver.io/pointer.html')
console.log(await browser.execute(() => (new Date()).toString()))
// returns "Thu Aug 01 2024 17:59:59 GMT-0700 (Pacific Daylight Time)"
```

Sie können die Systemzeit ändern, indem Sie [`setSystemTime`](/docs/api/clock/setSystemTime) oder [`tick`](/docs/api/clock/tick) aufrufen.