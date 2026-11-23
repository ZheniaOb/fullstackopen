# Part 0 — Fundamentals of Web apps

## 0.1–0.3 (reading only, not submitted)

- [ ] 0.1 [HTML basics](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics)
- [ ] 0.2 [CSS basics](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/CSS_basics)
- [ ] 0.3 [Your first form](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Your_first_HTML_form)

## 0.4 New note diagram

Page: https://studies.cs.helsinki.fi/exampleapp/notes — the user writes a note and clicks Save.

```mermaid
sequenceDiagram
    participant browser
    participant server

    Note right of browser: The user writes a note and clicks Save

    browser->>server: POST https://studies.cs.helsinki.fi/exampleapp/new_note
    activate server
    Note right of server: The server reads the note from the form body (note=...)<br/>and adds it with a date to the notes array
    server-->>browser: 302 Found, Location: /exampleapp/notes
    deactivate server

    Note right of browser: The browser follows the redirect and reloads the page

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/notes
    activate server
    server-->>browser: HTML document
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.css
    activate server
    server-->>browser: the css file
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.js
    activate server
    server-->>browser: the JavaScript file
    deactivate server

    Note right of browser: The browser starts executing main.js, which fetches the JSON from the server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate server
    server-->>browser: [{ "content": "new note for ex4", "date": "..." }, ...]
    deactivate server

    Note right of browser: The browser executes the callback that renders the notes, including the new one
```

## 0.5 Single page app diagram

Page: https://studies.cs.helsinki.fi/exampleapp/spa — the user opens the page.

```mermaid
sequenceDiagram
    participant browser
    participant server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/spa
    activate server
    server-->>browser: HTML document
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.css
    activate server
    server-->>browser: the css file
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/spa.js
    activate server
    server-->>browser: the JavaScript file
    deactivate server

    Note right of browser: The browser starts executing spa.js, which fetches the JSON from the server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate server
    server-->>browser: [{ "content": "HTML is easy", "date": "..." }, ...]
    deactivate server

    Note right of browser: The browser executes the callback that renders the notes (redrawNotes)
```

## 0.6 New note in Single page app diagram

Page: https://studies.cs.helsinki.fi/exampleapp/spa — the user creates a new note.

```mermaid
sequenceDiagram
    participant browser
    participant server

    Note right of browser: The user writes a note and clicks Save

    Note right of browser: spa.js prevents the default form submit,<br/>adds the note to the local notes list and re-renders it (redrawNotes)

    browser->>server: POST https://studies.cs.helsinki.fi/exampleapp/new_note_spa
    Note right of browser: Content-Type: application/json<br/>body: { "content": "...", "date": "..." }
    activate server
    Note right of server: The server adds the note to the notes array
    server-->>browser: 201 Created { "message": "note created" }
    deactivate server

    Note right of browser: No redirect and no page reload: the page stays as it is
```
