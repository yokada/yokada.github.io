---
layout: post
title: Create a image from movie using AV Foundation API
tags: objc
---

{% highlight objectivec linenos %}
#import <Foundation/Foundation.h>
#import <AVFoundation/AVFoundation.h>
#import <ImageIO/ImageIO.h>
#import <CoreServices/CoreServices.h>

int main(int argc, const char ** argv) {
    @autoreleasepool {
        AVAsset *asset = [AVAsset assetWithURL:[NSURL URLWithString:@"file:///path/to/movie.mov"]];
        AVAssetImageGenerator *ig = [AVAssetImageGenerator assetImageGeneratorWithAsset:asset];
        Float64 duration = CMTimeGetSeconds([asset duration]);
        CMTime midpoint = CMTimeMakeWithSeconds(duration/2.0, 600);
        NSError *err;
        CMTime actualTime;
        
        CGImageRef img = [ig copyCGImageAtTime: midpoint actualTime:&actualTime error:&err];
        
        NSString *path = [@"~/Desktop/test.png" stringByExpandingTildeInPath];
        NSLog(@"path: %@", path);
        CFURLRef url = (__bridge CFURLRef) [NSURL fileURLWithPath:path];
        
        CGImageDestinationRef dest = CGImageDestinationCreateWithURL(url, kUTTypePNG, 1, NULL);
        if (!dest) {
            NSLog(@"Failed to create CGImageDestination for %@", path);
            return -1;
        }
        
        CGImageDestinationAddImage(dest, img, nil);
        if (!CGImageDestinationFinalize(dest)) {
            NSLog(@"Failed to write image to %@", path);
            CFRelease(dest);
            return -1;
        }
        
        CFRelease(dest);
    }
    return 0;
}
{% endhighlight %}

----

## Resources

- [ AVFoundationPG PDF ]( https://developer.apple.com/jp/documentation/AVFoundationPG.pdf )
- [ objective c - Save CGImageRef to PNG file errors? (ARC Caused?) - Stack Overflow ]( http://stackoverflow.com/questions/8225838/save-cgimageref-to-png-file-errors-arc-caused )



