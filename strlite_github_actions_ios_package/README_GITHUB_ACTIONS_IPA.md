# Build iPhone IPA With GitHub Actions

This package builds an iOS IPA for the STRLite MNN model by reusing MNN's
official `project/ios` demo app. The included model is:

```text
deploy/strlite_video_t4_360x640.mnn
input:  frames [1, 4, 3, 360, 640]
output: erase  [1, 3, 360, 640]
output: mask   [1, 1, 360, 640]
```

## Recommended First Run: Unsigned IPA

1. Create a GitHub repository.
2. Upload this folder's contents to the repository root, including:
   - `.github/workflows/build-ios-mnn-demo.yml`
   - `deploy/strlite_video_t4_360x640.mnn`
3. Open GitHub `Actions`.
4. Run `Build STRLite MNN iOS IPA`.
5. Choose `build_mode = unsigned`.
6. Download the artifact `STRLite-MNN-demo-unsigned-ipa`.
7. Install it on Windows with Sideloadly or AltStore. Those tools will sign the
   app locally with your Apple ID.

This is the easiest route because GitHub Actions does not need your Apple
certificate or provisioning profile.

## Signed IPA Mode

Use this only if you have an Apple Developer certificate and provisioning
profile for the iPhone.

Add these GitHub repository secrets:

```text
APPLE_TEAM_ID
IOS_CERTIFICATE_P12_BASE64
IOS_CERTIFICATE_PASSWORD
IOS_PROVISION_PROFILE_BASE64
IOS_KEYCHAIN_PASSWORD
```

Then run the workflow with:

```text
build_mode = signed
bundle_id = your provisioning profile bundle id
```

## What This App Measures

The official MNN demo's `benchmark` button measures model inference latency on
device. It is useful for the Lite table's model-only FPS/latency.

For final paper numbers, still distinguish:

```text
model-only latency: MNN inference only
end-to-end latency: resize + normalization + MNN inference + output conversion
```

The current package is enough to start model-only iPhone 13 Pro Max testing.
