# SKFingerprintDemo
一个简单的指纹识别的Demo
### 调取指纹识别的条件 至少是5s以上 ，iOS sdk 至少是8.0 以上  
#### 所用到的框架就是 LocalAuthentication 这个框架  
##### 代码到时挺简单的。如下所示  
   
```  
- (void)bioFingerPrint {
    
    //1 初始化调用指纹方法的变量
    LAContext *lac = [[LAContext alloc]init];
    
    NSError *error = nil;
    
    // 2 判断设备是否拥有指纹识别的功能
    if ([lac canEvaluatePolicy:LAPolicyDeviceOwnerAuthenticationWithBiometrics error:&error]) {
        //3 调用方法
        // 1>LAPolicyDeviceOwnerAuthenticationWithBiometrics 就是代表生物授权的方法
        // 2>localizedReason 指纹解锁的时候显示的提示信息
        // 3> 调用回调，成功或者失败
        // 如果使用支付的话，支付的代码一般放在此block中,
        [lac evaluatePolicy:LAPolicyDeviceOwnerAuthenticationWithBiometrics localizedReason:@"放下您的金手指" reply:^(BOOL success, NSError * _Nullable error) {
            
            /**
             一般来讲,失败了，要去捕获失败的原因，一下是失败原因的类型
             
             1> 用户未提供有效证书（三次验证失败）
             LAErrorAuthenticationFailed = kLAErrorAuthenticationFailed,
             
             2> 用户取消
             LAErrorUserCancel           = kLAErrorUserCancel,
             
             3> 用户选择了输入密码
             LAErrorUserFallback         = kLAErrorUserFallback,
             
             4> 另外一个应用进入前台，比如系统来电,验证指纹界面直接消失
             LAErrorSystemCancel         = kLAErrorSystemCancel,
             
             5> 设备未设置密码，未设置密码是不能启用指纹识别的
             LAErrorPasscodeNotSet       = kLAErrorPasscodeNotSet,
             
             6> 设备指纹识别不可用
             LAErrorTouchIDNotAvailable  = kLAErrorTouchIDNotAvailable,
             
             7> 没有登记的手指触摸TouchID
             LAErrorTouchIDNotEnrolled = kLAErrorTouchIDNotEnrolled,
             
             
             
             */
            
            if (!error) { //指纹解锁成功 支付成功，success为1
                
                NSLog(@"指纹解锁成功,跳转支付。success:%d",success);
                
            }else{ //指纹解锁失败
                
                switch (error.code) {
                    case LAErrorAuthenticationFailed:
                    NSLog(@"用户未提供有效证书,三次验证失败");
                    break;
                    case LAErrorUserCancel:
                    NSLog(@"用户点击取消,比如点击回退按钮");
                    break;
                    case LAErrorUserFallback:
                    NSLog(@"用用户选择了输入密码");
                    break;
                    case LAErrorSystemCancel:
                    NSLog(@"另外一个应用进入前台，比如系统来电");
                    break;
                    case LAErrorPasscodeNotSet:
                    NSLog(@"设备未设置密码，未设置密码是不能启用指纹识别的");
                    break;
                    case LAErrorTouchIDNotAvailable:
                    NSLog(@"设备指纹识别不可用");
                    case LAErrorTouchIDNotEnrolled:
                    NSLog(@"没有登记的手指触摸TouchID");
                    break;
                    default:
                    break;
                }
                
                
            }
            
        }];
  
    }
    
}
```
