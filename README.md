# React Native 0.64.1 CodePush / Firebase example

> A working example of using the latest React Native CodePush with Firebase Performance Monitoring

## Running the app:

```bash
yarn # Install NPM packages
cd ios && pod install && cd ..  # Install iOS packages
npx react-native run-ios # Build and run in the simulator
```

## Running Metro:

If Metro doesn't automatically launch for some reason, manually run it in a separate terminal instance:

```bash
yarn start
```

### The problem:

When launching the app or is resumed and a new update is available from CodePush, the app attempts to download the update immediately and either will install it then or on next launch according to the release options. However, the app crashes at the download step. The only way to fix this seems to be removing Firebase Performance Monitoring.

```bash
# Example output:
[CodePush] Sync already in progress.
[CodePush] Checking for update.
[CodePush] Downloading package.
# App immediately crashes here
```
