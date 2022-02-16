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

## The problem:

When launching the app or is resumed and a new update is available from CodePush, the app attempts to download the update immediately and either will install it then or on next launch according to the release options. However, the app crashes at the download step. The only way to fix this seems to be removing Firebase Performance Monitoring.

```bash
# Example output:
[CodePush] Sync already in progress.
[CodePush] Checking for update.
[CodePush] Downloading package.
# App immediately crashes here
```

### XCode Error Output:

```
2022-02-16 14:24:00.132749-0700 Test[18623:6283080] [javascript] [CodePush] Checking for update.
2022-02-16 14:24:00.826170-0700 Test[18623:6282878] [boringssl] boringssl_metrics_log_metric_block_invoke(151) Failed to log metrics
2022-02-16 14:24:01.039669-0700 Test[18623:6283080] [javascript] [CodePush] Downloading package.
2022-02-16 14:24:01.048041-0700 Test[18623:6282878] -[fir_5913C783-5C89-4A5A-8CA5-2ED52A5D74F2_CodePushDownloadHandler _flex_swizzle_161f5b5f_connection:willSendRequest:redirectResponse:]: unrecognized selector sent to instance 0x6000007fc140
2022-02-16 14:24:01.059598-0700 Test[18623:6282878] *** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '-[fir_5913C783-5C89-4A5A-8CA5-2ED52A5D74F2_CodePushDownloadHandler _flex_swizzle_161f5b5f_connection:willSendRequest:redirectResponse:]: unrecognized selector sent to instance 0x6000007fc140'
*** First throw call stack:
(
	0   CoreFoundation                      0x000000010d17fba4 __exceptionPreprocess + 242
	1   libobjc.A.dylib                     0x000000010c28bbe7 objc_exception_throw + 48
	2   CoreFoundation                      0x000000010d18e811 +[NSObject(NSObject) instanceMethodSignatureForSelector:] + 0
	3   CoreFoundation                      0x000000010d1840ac ___forwarding___ + 1433
	4   CoreFoundation                      0x000000010d1861d8 _CF_forwarding_prep_0 + 120
	5   Test                                0x0000000100e0bb14 __62+[FLEXNetworkObserver injectWillSendRequestIntoDelegateClass:]_block_invoke.271 + 52
	6   Test                                0x0000000100e07492 +[FLEXNetworkObserver sniffWithoutDuplicationForObject:selector:sniffingBlock:originalImplementationBlock:] + 402
	7   Test                                0x0000000100e0b681 __62+[FLEXNetworkObserver injectWillSendRequestIntoDelegateClass:]_block_invoke_2 + 785
	8   CFNetwork                           0x000000010cce3cb5 kCFNIE + 11199
	9   CFNetwork                           0x000000010cd17ece __CFTubeSetTubeTypeNotifier + 106682
	10  Foundation                          0x000000010dd2e4bb __NSBLOCKOPERATION_IS_CALLING_OUT_TO_A_BLOCK__ + 7
	11  Foundation                          0x000000010dd2e3b3 -[NSBlockOperation main] + 98
	12  Foundation                          0x000000010dd313c3 __NSOPERATION_IS_INVOKING_MAIN__ + 17
	13  Foundation                          0x000000010dd2d600 -[NSOperation start] + 785
	14  Foundation                          0x000000010dd31d17 __NSOPERATIONQUEUE_IS_STARTING_AN_OPERATION__ + 17
	15  Foundation                          0x000000010dd3185c __NSOQSchedule_f + 182
	16  libdispatch.dylib                   0x000000010eb93a76 _dispatch_block_async_invoke2 + 83
	17  libdispatch.dylib                   0x000000010eb84a2c _dispatch_client_callout + 8
	18  libdispatch.dylib                   0x000000010eb8b3a6 _dispatch_lane_serial_drain + 845
	19  libdispatch.dylib                   0x000000010eb8c0bc _dispatch_lane_invoke + 436
	20  libdispatch.dylib                   0x000000010eb98472 _dispatch_workloop_worker_thread + 900
	21  libsystem_pthread.dylib             0x000000011023045d _pthread_wqthread + 314
	22  libsystem_pthread.dylib             0x000000011022f42f start_wqthread + 15
)
libc++abi: terminating with uncaught exception of type NSException
dyld4 config: DYLD_ROOT_PATH=/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot DYLD_LIBRARY_PATH=/Users/derekdinsmore/Library/Developer/Xcode/DerivedData/Test-cyabcvcrwzskxvhewcxuatquvpmw/Build/Products/Debug-iphonesimulator:/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/lib/system/introspection DYLD_INSERT_LIBRARIES=/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/lib/libBacktraceRecording.dylib:/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/usr/lib/libMainThreadChecker.dylib:/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Library/Developer/CoreSimulator/Profiles/Runtimes/iOS.simruntime/Contents/Resources/RuntimeRoot/Developer/Library/PrivateFrameworks/DTDDISupport.framework/libViewDebuggerSupport.dylib DYLD_FRAMEWORK_PATH=/Users/derekdinsmore/Library/Developer/Xcode/DerivedData/Test-cyabcvcrwzskxvhewcxuatquvpmw/Build/Products/Debug-iphonesimulator
*** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '-[fir_5913C783-5C89-4A5A-8CA5-2ED52A5D74F2_CodePushDownloadHandler _flex_swizzle_161f5b5f_connection:willSendRequest:redirectResponse:]: unrecognized selector sent to instance 0x6000007fc140'
terminating with uncaught exception of type NSException
CoreSimulator 783.5 - Device: iPhone 12 (1E944067-C473-49CC-A0C9-E3A9B021D136) - Runtime: iOS 15.2 (19C51) - DeviceType: iPhone 12
```
