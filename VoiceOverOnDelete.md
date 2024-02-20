## How to start SwiftUI List onDelete action with VoiceOver?
### Source:
https://stackoverflow.com/questions/78023101/how-to-start-swiftui-list-ondelete-action-with-voiceover

## Answer

You need to apply `onDelete` modifier, and SwiftUI will do the rest by providing Voice Over user with the next hint: 
“<Row element’s label, value, etc.>. Swipe up or down to select a custom action. Then double tap to activate.”

Even though you add `EditButton()` to your toolbar VoiceOver won’t allow you to interact with delete buttons(the red one with minus sign).

I assume it was made intentionally, to avoid making additional effort for VO user.
