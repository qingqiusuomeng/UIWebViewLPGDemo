# UIWebViewLPGDemo
不同于WKWebview，wk是有自己的加载进度值的，我们可以直接通过kvo检测到，并显示到进度条内。

但如果我们为了适配ios7，只能使用UIWebview了，这里的加载进度，就比较尴尬了（WKWebview性能还是比UIwebView好很多，）

所以我们的实现方式就是：模拟进度-俗称假进度。
//进度条
    _progressView = [[UIProgressView alloc] initWithFrame:CGRectMake(0, 62, bgView.bounds.size.width, 2)];
    _progressView.progressTintColor = [UIColor colorWithHexString:GrassGreen];
    _progressView.trackTintColor = [UIColor colorWithHexString:GrayLightColor];
    [bgView addSubview:_progressView];


//UIWebViewDelegate
-(void)webViewDidStartLoad:(UIWebView *)webView{
    _progressView.progress = 0;
    _isLoadFinish = NO;
    _timer = [NSTimer scheduledTimerWithTimeInterval:0.01667 target:self selector:@selector(timerCallback) userInfo:nil repeats:YES];
    
}
-(void)webViewDidFinishLoad:(UIWebView *)webView{
    _isLoadFinish = YES;
}


-(void)webView:(UIWebView *)webView didFailLoadWithError:(NSError *)error{
    
}


-(void)timerCallback {
    if (_isLoadFinish) {
        if (_progressView.progress >= 1) {
            _progressView.hidden = true;
            [_timer invalidate];
        }else {
            _progressView.progress += 0.1;
        }
    }else {
        _progressView.progress += 0.05;
        if (_progressView.progress >= 0.95) {
            _progressView.progress = 0.95;
        }
    }
}

页面消失的时候记得销毁定时器
