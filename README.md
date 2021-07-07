# Android App Deployment with Fastlane + GitHub Actions CI / CD

A sample repository to demonstrate the Automate publishing app to the _Google Play Store_ with GitHub Actions⚡+ Fastlane🏃.  

## Refer these articles

> These article includes all steps or related information for setting up this.

- https://medium.com/firebase-developers/quickly-distribute-app-with-firebase-app-distribution-using-github-actions-fastlane-c7d8eca18ee0
- https://medium.com/scalereal/automate-publishing-app-to-the-google-play-store-with-github-actions-fastlane-ac9104712486

## Status

| Deployment Status (Production)                                                                                             | Deployment Status (Beta)                                                                                       | Distribution Status |
|----------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------| -------------------- |
| ![Release (Production)](https://github.com/PatilShreyas/AndroidFastlaneCICD/workflows/Release%20(Production)/badge.svg)    | ![Release (Beta)](https://github.com/PatilShreyas/AndroidFastlaneCICD/workflows/Release%20(Beta)/badge.svg)    | ![Distribute](https://github.com/PatilShreyas/AndroidFastlaneCICD/workflows/Distribute/badge.svg) |

---

## Workflows

- [`distribute.yml`](.github/workflows/distribute.yml) - For Distributing test builds using the Firebase App Distribution.
- [`releaseBeta.yml`](.github/workflows/releaseBeta.yml) - For deploying application (AAB) to the _beta_ track on to the Google Play Store.
- [`releaseProd.yml`](.github/workflows/releaseProd.yml) - For deploying application (AAB) to the _production_ track on to the Google Play Store.

## Fastlane config

#### - Fastfile

```ruby
    desc "Lane for distributing app using Firebase App Distributions"
    lane :distribute do
        gradle(task: "clean assembleRelease")
        firebase_app_distribution(
            service_credentials_file: "firebase_credentials.json",
            app: ENV['FIREBASE_APP_ID'],
            release_notes_file: "FirebaseAppDistributionConfig/release_notes.txt",
            groups_file: "FirebaseAppDistributionConfig/groups.txt"
        )
    end
    
    desc "Deploy a beta version to the Google Play"
    lane :beta do
        gradle(task: "clean bundleRelease")
        upload_to_play_store(track: 'beta', release_status: 'draft')
    end

    desc "Deploy a new version to the Google Play"
    lane :production do
        gradle(task: "clean bundleRelease")
        upload_to_play_store(release_status: 'draft')
    end
```

## References

- [Fastlane](https://fastlane.tools/)
- [GitHub Actions CI/CD](https://github.com/features/actions)
- [Firebase App Distribution](https://firebase.google.com/docs/app-distribution)
