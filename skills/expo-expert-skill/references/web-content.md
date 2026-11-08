# Web Content and In-App Browser (Expo)

SwiftUI idea: WebKit integration covers web views, navigation,
JavaScript, and exports. The Expo form is `expo-web-browser` for
opening links and `react-native-webview` for embedded pages.
Concepts only — check the versioned Expo docs before writing code.

## Opening a link

- Use `expo-web-browser` to open an external link in an in-app
  browser. Check the versioned Expo docs for the current API.
- A link tap must never fail silently. If opening fails, show
  the URL with a Copy Link option. This is a standing house rule.
- Keep the user in the app for support, terms, and privacy pages
  where an in-app browser fits. Use the system browser only when
  the page needs it.

## Embedded web views

- Use `react-native-webview` only when the content must live
  inside the app. A link that could open in the browser should
  open in the browser.
- An embedded web view needs a loading state, an error state,
  and a close action. A blank web view is a bug.
- Do not inject JavaScript you have not reviewed. Do not pass
  secrets into a web view.

## Navigation

- A web view that can navigate needs a back control that works.
  The system back gesture must leave the web view, not the app.
- External links inside a web view should open in the browser,
  not trap the user inside the embedded page.

## Checklist

- [ ] Links open with `expo-web-browser` unless embedded is needed.
- [ ] Failed link opens show the URL with Copy Link.
- [ ] Embedded views have loading, error, and close states.
- [ ] No secrets are passed into a web view.
- [ ] Back leaves the web view before it leaves the app.

## Worked example: terms and privacy

The paywall has "Terms of Use" and "Privacy Policy" links. Both
open in the in-app browser. If opening fails, the app shows the
URL with a Copy Link option. That is the link-tap rule, and it
applies to every external link, not just these two.

- The user stays in the app. The paywall is still there when
  they come back.
- A failed open is visible. The user sees the URL and can copy it.
- The link text says where it goes: "Terms of Use", not "here".

## Common mistakes

- A link that does nothing when tapped, silently. This is the
  bug the house rule exists for. Never ship it.
- An embedded web view for a page that could open in the browser.
  The web view needs loading, error, and close states, and it
  still feels worse than the browser.
- A web view with no close action. The user is trapped on a web
  page inside the app.
- Secrets passed into a web view: a token in a URL, a key in
  injected JavaScript. The web page can read what you pass it.
- A web view that opens external links inside itself. The user
  taps a link and lands on another site, still inside the app,
  with no way back except the app's back button.

## Choosing

- Support, terms, privacy, and help: in-app browser.
- A page the app must control, such as an embedded checkout the
  docs require: web view, with loading, error, and close.
- Anything else: browser first. Embedded only with a reason.
## Errors in a web view

- A web view that fails to load shows an error with a retry action
  and a way out. It never shows a blank white screen.
- The error says what happened in plain words: "This page could
  not be loaded. Check your connection and try again." It does not
  show a raw error code as the whole message.
- A retry reloads the page. A close leaves the web view entirely.
