# Talk Anywhere Privacy Policy

Talk Anywhere is designed so that your conversations stay on your device.

## Default: nothing is saved

Translation sessions are held in memory only. When you close a session, it is
discarded. There is no account, no cloud sync, and no server operated by this
project.

## Opt-in history

Saving history is an explicit toggle in Settings, **off by default**, with a
plain-language explanation of what is stored (on this device only). When enabled:

- Transcript text (original and translated) is never stored as plaintext. Turn
  text is encrypted with CryptoKit AES-GCM using a per-install symmetric key kept
  in the iOS Keychain. The store file also inherits iOS's default file
  protection (data protected until first device unlock); the app does not set a
  stronger protection class than the OS default.
- You choose a retention period: delete after session close / 24 hours / 7 days /
  30 days / never. **The default is 7 days.** Expired sessions are swept at app
  launch and session close.
- **Delete All History** takes effect immediately and also destroys and
  regenerates the encryption key (cryptographic erasure) — previously written
  data becomes unreadable even if storage remnants exist.

## Network use

Once the required models are installed, translation and text-to-speech run
**fully on-device** using Apple frameworks, Google ML Kit, and the bundled
offline models. Your conversation text is translated locally and is not
uploaded for translation. The app does, however, make network requests in a few
specific, user-initiated situations, described below. It is important to
distinguish **model downloads** (which use the network) from **translation
itself** (which happens on-device).

- **Speech recognition.** When on-device recognition is unavailable for a
  language, Apple's Speech framework sends the audio to Apple's servers to
  perform the recognition. The diagnostics screen always shows whether
  recognition is on-device or server-based for the current language, and the
  **Offline readiness** screen (Settings → Languages) says so for your current
  pair *before* you speak, so you can see it in advance rather than after the
  fact.
- **Offline translation packs (Hugging Face).** When you explicitly choose to
  download an offline translation pack, the app makes its own network requests
  to Hugging Face's servers (`huggingface.co`) to fetch the NLLB model files.
  This happens only when you initiate a pack download; no request is made
  otherwise. Once downloaded, translation with these packs runs on-device.
- **Google ML Kit translation models.** Talk Anywhere uses Google ML Kit (see
  "Third-party SDKs" below) for a tier of supported languages. When you select
  a language served by Google ML Kit, the SDK downloads the corresponding
  translation model from Google's servers on demand. The translation itself
  then runs on-device; your text is not sent to Google to be translated.
- **Camera currency conversion.** If you enable price conversion, the app
  downloads public daily rates from Frankfurter and saves them for offline use.
  No camera text, images, location, or currency preferences are sent. See
  "Camera and photos" below for details.

Downloading an Apple language pack for translation or recognition is performed
by iOS through Apple's system download flow, not by this app.

## Text shared in from other apps

When you share selected text into Talk Anywhere from another app, that text moves
through a private container shared only between Talk Anywhere and its own share
extension (an iOS app group). It does not travel through a link that other apps
on your device could read or forge, and it never leaves your device. Text
arriving through the `talkanywhere://translate?...` URL scheme is likewise handled
entirely on-device.

## What we never do

- No analytics or product telemetry of our own. Google ML Kit can report usage
  and diagnostics to Google by default; Talk Anywhere ships with that telemetry
  explicitly disabled (the `MLKitDisableTelemetry` key is set in the app's
  Info.plist).
- No accounts or sign-in.
- No sharing of any data unless you explicitly use the system share sheet
  (per-turn copy/share or share-session-as-text), which you fully control.

## Third-party components

Talk Anywhere includes **Google ML Kit** (the `GoogleMLKit/Translate` component)
for a tier of supported languages and **WhisperKit** for local speech
recognition. ML Kit downloads translation models from Google when needed; the
translation itself then runs on-device. ML Kit's default usage and diagnostics
reporting is disabled in this app. The Whisper Base and Small models ship in the
app and run locally.

## Camera and photos

Talk Anywhere can recognize text from the live camera and from photos you import.
Camera frames and imported images are processed **on-device** to extract text
for translation. Images are not stored by the app and are not uploaded.

Live Camera and Photo to Translate offer optional price conversion, off until you enable it in
**Prices & units**. Prices are detected and converted on-device. When enabled,
the app downloads daily public exchange rates from `api.frankfurter.dev` over
HTTPS. Saved rates are refreshed when older than a day, or when you tap
**Refresh rates**. Failed automatic updates may retry when reopening a camera/photo screen,
with at least a minute between attempts in a session.
This request contains no camera text, images, location, or currency preferences.
Like any web service, Frankfurter receives the connection's IP address.
Rates, their dates, and your currency settings are saved locally so conversion
can work offline. Scanned prices are not persisted by this feature. Saved rates
may be out of date; the Prices & units sheet shows each conversion's rate date.
Conversions are estimates and do not include payment-provider fees.
Both modes share preferences and the rate cache. Imported photo text, receipt
positions, and currency-context analysis stay on-device and are not sent to the
rate service. Clearing/replacing a photo clears its in-memory conversion input.

Measurement conversion is enabled by default and works entirely on-device,
without any network requests. The unit-system preference and confirmed meanings
of ambiguous unit markers are stored locally. This feature does not persist
scanned measurements. It can be disabled separately from currency conversion.

## Permissions

The app requests microphone and speech-recognition permissions with honest usage
descriptions, only after explaining why on the onboarding screen. Denying either
permission leads to a recovery screen with a link to Settings — the app never
nags in a loop.

## Microphone behavior

On the Speak screen, the default mic control is tap once to start and tap again
to stop; an optional hold-to-speak style records only while held. Conversation's
Tap to Talk mode uses explicit turns: Side by Side uses the same tap-to-toggle
control, while Face to Face records while a person's button is held. In Live
mode the app keeps capture active and starts each recognition turn automatically
until you stop or pause the session. With Continue in background enabled, Live
can keep the microphone active while the screen is locked and auto-pauses after
the configured period of silence. The system microphone indicator remains
visible whenever capture is active. Audio is not stored or transmitted by the
app, though Apple may process recognition on its servers when an on-device
recognizer is unavailable, as described under Network use.
