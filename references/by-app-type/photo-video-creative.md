# Photo, Video & Creative — App-Type Checklist

**Covers:** Photo & Video, Graphics & Design, Video editors, Camera apps, Wallpapers, Drawing/painting, Fonts, AI image generators (see also `ai-apps.md`).

## Top rejection reasons

| Risk | Guideline | Detail |
|---|---|---|
| Full Photos library access where a picker suffices | 5.1.1(iii) | prefer `PHPickerViewController`/share sheet; justify any `PHPhotoLibrary` full access |
| Purpose strings vague | 5.1.1(ii) | "PhotoSync needs access to save your edited photos" — specific + app-named |
| Permission bundling | 5.1.1(iv) | filters must not require camera; export must not require mic |
| App works when denied? | 5.1.1(iv) | after denial, degrade gracefully (manual picks, sample assets) |
| NSFW generation without filters | 1.1.4, 1.2 | AI image tools need output moderation + mature content handling |
| Wallpaper/simple-filter spam | 4.3(b) | saturated category — needs meaningfully different experience |
| Photo-rating/objectification mechanics | 1.2 escalation | hot-or-not on real people's photos |
| Contacts-style harvesting via share sheets | 5.1.2(iv)(v) | no contact DB building; no select-all invites |
| Copyrighted fonts/assets/stickers | 5.2.1 | rights for bundled assets |
| Face-mapping data misuse | 5.1.2(vi) | ARKit/depth/face data never for ads/profiling |
| Recording without indicator | 2.5.14 | screen/camera capture with visible/audible consent indicator |

## Audit specifics

- **Permission economy:** list every protected resource (photos add/save, camera, mic, music library) × actual feature need × denial-path behavior. Editors commonly over-ask at first launch — lazy onboarding = 5.1.1(iii)/(iv) findings.
- **NSFW policy:** if the app can produce/display mature imagery: classifier on output, mature section off by default, age rating aligned (17+), and if UGC-shared → full 1.2 stack (`social-ugc.md`).
- **Export/share paths:** file picker includes Files + iCloud (2.5.15); share sheet usage over manual access.
- **Camera apps:** `AVCaptureDevice` purpose strings; background camera use only with proper background modes justification (2.5.4); recording indicator (2.5.14).
- **Watermark/logo removal tools:** marketed as "remove watermarks" → 5.2-adjacent facilitation rejection; reframe to licensed-content editing.
- **Live captions/face filters on minors:** extra care (5.1.4 posture if children-primary).

## Pre-submission checklist

- [ ] Pickers/share sheets preferred over full library access
- [ ] Every purpose string specific; denial paths still usable
- [ ] AI/output moderation present; mature content off by default; rating aligned
- [ ] No objectification mechanics; recording indicators present
- [ ] Rights for all bundled fonts/stickers/assets
- [ ] Saturated-category differentiation visible in first-run experience
