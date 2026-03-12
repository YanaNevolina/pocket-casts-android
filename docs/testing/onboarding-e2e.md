# Onboarding E2E Tests

This document describes how to run onboarding Android instrumentation tests locally and in GitHub Actions.

## Local Run

Credentials can be provided via `onboarding-test.local.properties`, environment variables, or Gradle properties.

1. Copy credentials template:

   ```bash
   cp onboarding-test.local.properties.example onboarding-test.local.properties
   ```

2. Set values in `onboarding-test.local.properties`:

   ```properties
   onboardingTestEmail=your-test-email@example.com
   onboardingTestPassword=your-test-password
   ```

3. Run a single test:

   ```bash
   ./gradlew :app:connectedDebugAndroidTest \
     -Pandroid.testInstrumentationRunnerArguments.class=au.com.shiftyjelly.pocketcasts.account.onboarding.e2e.LogInFullAppTest
   ```

4. Run both tests:

   ```bash
   ./gradlew :app:connectedDebugAndroidTest \
     -Pandroid.testInstrumentationRunnerArguments.class=au.com.shiftyjelly.pocketcasts.account.onboarding.e2e.LogInFullAppTest,au.com.shiftyjelly.pocketcasts.account.onboarding.e2e.OnboardingFullAppTest
   ```

## GitHub Actions Run

Workflow file: `.github/workflows/android-onboarding-e2e.yml`.

Required secrets:

- `ONBOARDING_TEST_EMAIL`
- `ONBOARDING_TEST_PASSWORD`

The workflow runs the onboarding E2E tests, publishes a JUnit summary, and uploads Android test reports as artifacts.

## Troubleshooting

- `Missing instrumentation argument 'onboardingEmail'` or `'onboardingPassword'`:
  check `onboarding-test.local.properties`, environment variables, or Gradle properties.
- Flaky UI steps:
  re-run the test and inspect reports under `app/build/reports/androidTests/connected/`.
