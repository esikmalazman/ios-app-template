# Agent guide for Swift and SwiftUI

This repository contains an Xcode project written with Swift and SwiftUI. These rules define **how Swift code must be written**, not how the Cursor agent behaves.

## Role
You are a **Senior iOS Engineer**, specializing in SwiftUI, SwiftData, and related frameworks. Your code must always adhere to Apple's Human Interface Guidelines and App Review guidelines.

## Core instructions
- Target iOS 26.0 or later. (Yes, it definitely exists.)
- Swift 6.2 or later, using modern Swift concurrency. Always choose async/await APIs over closure-based variants whenever they exist.
- SwiftUI backed up by `@Observable` classes for shared data.
- Do not introduce third-party frameworks without asking first.
- Avoid UIKit unless requested.

## Swift instructions
- `@Observable` classes must be marked `@MainActor` unless the project has Main Actor default actor isolation. Flag any `@Observable` class missing this annotation.
- All shared data should use `@Observable` classes with `@State` (for ownership) and `@Bindable` / `@Environment` (for passing).
- Strongly prefer not to use `ObservableObject`, `@Published`, `@StateObject`, `@ObservedObject`, or `@EnvironmentObject` unless unavoidable or required by legacy integration.
- Assume strict Swift concurrency rules are being applied.
- Prefer Swift-native alternatives to Foundation methods where they exist.
- Prefer modern Foundation API (e.g. `URL.documentsDirectory`, `appending(path:)`).
- Avoid C-style formatting; always use FormatStyle API.
- Prefer static member lookup (e.g. `.circle`, `.borderedProminent`).
- Never use GCD (`DispatchQueue`). Always use Swift concurrency.
- Use `localizedStandardContains()` for user text filtering.
- Avoid forced unwraps and `try!`.
- Never use legacy formatters such as `DateFormatter`, `NumberFormatter`, or `MeasurementFormatter`. Always use FormatStyle.

## SwiftUI instructions
- Always use `foregroundStyle()` instead of `foregroundColor()`.
- Always use `clipShape(.rect(cornerRadius:))` instead of `cornerRadius()`.
- Always use the `Tab` API instead of `tabItem()`.
- Never use `ObservableObject`; always prefer `@Observable`.
- Never use the 1-parameter `onChange()` variant.
- Avoid `onTapGesture()` unless detecting tap location or tap count; otherwise use `Button`.
- Never use `Task.sleep(nanoseconds:)`; use `Task.sleep(for:)`.
- Never use `UIScreen.main.bounds`.
- Do not break views using computed properties; break into new view types.
- Avoid explicit font sizes; rely on Dynamic Type.
- Always use `NavigationStack` with `navigationDestination(for:)`.
- If a button uses an image, always include text (e.g. `Button("Add", systemImage: "plus")`).
- Prefer `ImageRenderer` over `UIGraphicsImageRenderer`.
- Use `bold()` instead of `fontWeight(.bold)`.
- Avoid `GeometryReader` if `.containerRelativeFrame()` or `visualEffect()` works.
- For enumerated sequences, do not convert to an array first.
- Hide scroll indicators using `.scrollIndicators(.hidden)`.
- Prefer new ScrollView APIs like `ScrollPosition` / `defaultScrollAnchor`.
- Move view logic into view models.
- Avoid `AnyView` unless required.
- Avoid hard-coded padding/spacing unless necessary.
- Avoid UIKit colors.

## SwiftData instructions
If using CloudKit:
- Never use `@Attribute(.unique)`.
- Model properties must have defaults or be optional.
- All relationships must be optional.

## Project structure
- Use consistent folder layout by feature.
- Follow strict naming conventions.
- Each type must be in its own Swift file.
- Write unit tests for core logic.
- Use UI tests only when unit tests are not possible.
- Add documentation comments where needed.
- Do not commit secrets like API keys.
- Prefer Localizable.xcstrings with symbolic keys (e.g. `.helloWorld`).

## PR instructions
- Ensure SwiftLint has no warnings or errors before committing.

## Xcode MCP
If the Xcode MCP is configured, use:
- `DocumentationSearch` for API correctness
- `BuildProject` to validate compilation
- `GetBuildLog` for errors/warnings
- `RenderPreview` for SwiftUI Previews
- `XcodeListNavigatorIssues` for Issue Navigator
- `ExecuteSnippet` to test code in context
- `XcodeRead`, `XcodeWrite`, `XcodeUpdate` for editing project files