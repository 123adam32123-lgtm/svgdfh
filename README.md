# StudyLens

StudyLens is a native iOS 17+ SwiftUI app for learner-initiated analysis of educational images. It uses a user-provided Gemini API key to explain concepts, show a worked approach, and present a labelled suggested solution.

## Build

1. On a Mac, open `StudyLens.xcodeproj` in Xcode and choose a signing team for both targets.
2. In **Signing & Capabilities**, create/register the App Group `group.com.example.StudyLens` and enable it for both targets. If you change the bundle identifier, change this group identifier consistently in `ShareSheetService.swift`, `ShareViewController.swift`, and both entitlement files.
3. Add an App Icon image to `StudyLens/Assets.xcassets/AppIcon.appiconset` before distributing the app.
4. Build on a physical iPhone to use the camera; the simulator has no camera.

`project.yml` is included as an optional XcodeGen definition for maintaining the project structure.

## Build without a Mac

The included `.github/workflows/build-unsigned-ipa.yml` can build an **unsigned** `StudyLens-unsigned.ipa` on a GitHub-hosted macOS runner. Create a GitHub repository, upload this project, open **Actions**, select **Build unsigned IPA**, and click **Run workflow**. Download the `StudyLens-unsigned-ipa` artifact after the workflow succeeds.

An unsigned IPA is useful only as a build artifact. iOS will not install it unless a later tool signs it with an Apple certificate and provisioning profile.

The project uses the Gemini `generateContent` REST endpoint with the `x-goog-api-key` header. The default model is `gemini-3.5-flash-lite`, but the model identifier is editable in Settings so it can be updated when Google changes model availability. See Google’s [GenerateContent reference](https://ai.google.dev/api/generate-content) and [image understanding guide](https://ai.google.dev/gemini-api/docs/image-understanding).

## Privacy

- The API key is stored in the iOS Keychain with `kSecAttrAccessibleWhenUnlockedThisDeviceOnly`.
- Images are only sent after the user taps **Analyze Selected Image**; the app does not persist submitted images.
- Shared images are written into the App Group container only long enough for the main app to consume them, then deleted.
- Gemini receives the selected image and study prompt directly. Review Google’s data practices before using the app with sensitive material.

## iOS limitations

Third-party iOS apps cannot intercept global volume buttons, silently screenshot other apps, or place an unrestricted overlay above other apps. StudyLens therefore supports only explicit Photos, in-app Camera, and share-extension image input, and presents results inside its own SwiftUI sheet.
