# Intent — Results-ready notification

Written from the approved PRD N-17 (2026-09-18) and the discovery notes of 2026-09-20.
The early request of 2026-08-02 is marked superseded and is treated as history, not as
approval: nothing below rests on it.

## Problem

- **I-1** Patients cannot tell when a result is ready for them, so they call the clinic to
  ask. These status-only calls are avoidable: the information already exists in the
  clinic's systems, it simply never reaches the patient.
- **I-2** The absence of a signal, not the portal itself, is the problem. The existing
  authenticated portal already holds released results; patients just have no reason to
  open it at the right moment.

## Proposed outcome

- **I-3** When a doctor has reviewed and released a result, notify the patient that an
  update is available and direct them to the existing authenticated portal. The result
  itself stays behind authentication.
- **I-4** Success will be observed as a reduction in **status-only calls** — counted
  separately from clinical questions, per the support lead — and a low and falling rate of
  **delivery failures**. Both are measured against a baseline agreed during discovery.
- **I-5** No numeric target is set here. The PRD states that the baseline and numerical
  target will be agreed during discovery and that none is approved yet, so this intent
  records the direction of travel and the measures, not a percentage.

## Affected users and systems

- **I-6** Patients awaiting a released result — the recipients of the notification.
- **I-7** Doctors, whose review-and-release action becomes the trigger. The notification
  must not change how or when they review.
- **I-8** Support staff, who answer the calls today and who provide the call-volume
  measurement.
- **I-9** Systems: the existing authenticated portal and its existing result route, the
  result release step, and the notification delivery path and its logs.

## Out of scope

- **I-10** Portal redesign and any new patient dashboard. The engineering lead records a
  new dashboard as a separate project.
- **I-11** Appointment scheduling.
- **I-12** Clinical interpretation of the result, in the message or anywhere else.
- **I-13** Notifying third parties. Referring-doctor notification is **not** silently
  included here: it is excluded from approved scope *and* carried below as an open
  question with its own owners, because no source authorizes the recipient.
- **I-14** Attaching results to the message, and any public or unauthenticated result
  link.
- **I-15** Anything approved only by the superseded 2026-08-02 request: notifying on lab
  upload, putting the result value in an SMS, copying the referring doctor, rebuilding the
  dashboard, and the 90%-in-one-week figure.

## Constraints

- **I-16** A doctor must review **and** release the result before any notification is
  eligible. "Ready" must not mean that the laboratory upload merely finished.
- **I-17** The message says only that an update is available and points to the portal. No
  diagnosis, test result, clinical measurement or treatment detail appears in the message
  **or in notification logs**.
- **I-18** Access stays authenticated in the existing portal; use the existing result
  route.
- **I-19** No duplicate notification for the same released result version, and delivery
  failures are handled without sending the patient a duplicate message.
- **I-20** Recipients beyond the patient are not approved and must not be added without an
  explicit authorization decision.

## Open questions

- **OPEN-1 Channel.** SMS, email or portal push is undecided, pending the consent and
  delivery review. **Owners: product owner and privacy owner.**
- **OPEN-2 Referring-doctor notification.** Whether the referring doctor is notified at
  all is undecided; it turns on recipient authorization and is not approved by the PRD.
  **Owners: clinical owner and privacy owner.**

Both questions must be closed by their owners before specification fixes the behaviour.
Until then, the intent assumes the patient is the only recipient and leaves the channel
unspecified.

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
