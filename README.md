# Google Objective-C 스타일 가이드

## 배경

Objective-C는 C언어를 객체지향으로 확장 시킨 언어로 매우 동적이다. Objective-C는 객체 지향 설계에 능숙한 사람이라면 사용하기 쉽고 읽기 쉽도록 설계되었다. Mac OS X와 iPhone을 개발하는데 사용되는 주 언어이기도 하다.
코코아는 Mac OS X의 메인 프레임워크중 하나이다. 코코아는 Mac OSX의 모든 기능을 빠르게 개발할 수 있도록 만든 Objective-C 클래스들이다.
애플은 벌써 잘되있고 널리 통용되는 Objective-C 코딩 가이드를 만들었다. 구글 또한 C++ 코딩가이드와 유사하게 Objective-C 코딩 가이드를 만들었다. 이 Objective-C 가이드는 애플의 가이드와 자연스럽게 어울리는데 목표를 두었다. 그래서 먼저 아래의 가이드를 읽어보면 좋을것 같다.

* [애플 코코아 코딩 가이드라인](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [구글 오픈소스 C++ 스타일 가이드](http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml)

이 가이드는 Objective-C(Objective-C++ 포함) 코딩 가이드를 설명하고 또 모든 Mac OS X 코드에 사용하게 하는데 목적이있다. 많은 프로젝트와 팀에서 이 가이드 라인이 사용되고 입증되었다. 구글에서 개발한 오픈 소스 프로젝트에는 이 가이드 라인을 따른다.
구글은 이 가이드를 따라 작성된 Google Toolbox for Mac(줄여서 GTM) 프로젝트를 내놓았다. 
이 가이드는 Objective-C 튜토리얼은 아니다. 이 가이드는 Objective-C를 다룰 줄 아는 것을 가정한다.

## 예제

헤더 파일의 예제이다. @interface에 올바른 주석과 공백의 예제를 보여준다.

	#import <Foundation/Foundation.h>

	// A sample class demonstrating good Objective-C style. All interfaces,
	// categories, and protocols (read: all top-level declarations in a header)
	// MUST be commented. Comments must also be adjacent to the object they're
	// documenting.
	//
	// (no blank line between this comment and the interface)
	@interface Foo : NSObject {
	 @private
	  NSString *_bar;
	  NSString *_bam;
	}
	
	// Returns an autoreleased instance of Foo. See -initWithBar: for details
	// about |bar|.
	+ (id)fooWithBar:(NSString *)bar;
	
	// Designated initializer. |bar| is a thing that represents a thing that
	// does a thing.
	- (id)initWithBar:(NSString *)bar;
	
	// Gets and sets |_bar|.
	- (NSString *)bar;
	- (void)setBar:(NSString *)bar;
	
	// Does some work with |blah| and returns YES if the work was completed
	// successfully, and NO otherwise.
	- (BOOL)doWorkWithBlah:(NSString *)blah;
	
	@end


소스 파일의 예제이다. @implementation에 올바를 주석과 공백의 예제를 보여준다. 또 init, dealloc, getter, setter등 주요 메서드에 대한 예제도 포함하고 있다.

	#import "Foo.h"

	@implementation Foo
	
	+ (id)fooWithBar:(NSString *)bar {
	  return [[[self alloc] initWithBar:bar] autorelease];
	}
	
	// Must always override super's designated initializer.
	- (id)init {
	  return [self initWithBar:nil];
	}
	
	- (id)initWithBar:(NSString *)bar {
	  if ((self = [super init])) {
	    _bar = [bar copy];
	    _bam = [[NSString alloc] initWithFormat:@"hi %d", 3];
	  }
	  return self;
	}
	
	- (void)dealloc {
	  [_bar release];
	  [_bam release];
	  [super dealloc];
	}
	
	- (NSString *)bar {
	  return _bar;
	}
	
	- (void)setBar:(NSString *)bar {
	  [_bar autorelease];
	  _bar = [bar copy];
	}
	
	- (BOOL)doWorkWithBlah:(NSString *)blah {
	  // ...
	  return NO;
	}
	
	@end

@interface, @implemetation, @end 다음에 오는 빈줄은 선택사항이다. 만약 @interface에 인스턴스 변수를 선언했다면, 닫는 중괄호 (}) 다음에 빈줄이 와야한다. 
private 메서드나 bridge 클래스 등을 선언하여 interface나 implementation이 매우 짧은 경우가 아니라면 빈줄은 코드를 읽은데 많은 도움이 된다.

## 공백과 포멧팅

#### 공백 vs 탭

공백만 사용하며 들여쓰기는 2개의 공백을 사용한다. 탭을 사용하지 않기 때문에 에디터에서 탭을 공백으로 바꾸는 설정을 해줘야한다.

#### 라인 길이

한 줄의 길이는 최대 80자를 넘으면 안된다. 80자 제한은 더 읽기 좋다.
XCode > Preferences > Text Editing > Show page guide 를 설정하면 80자의 경계선을 보여준다.

#### 메서드 선언 및 정의

\- 또는 + 다음에 공백 하나를 포함하고 리턴 타입을 작성고 파라미터 리스트에는 공백을 두지 않는다.

	- (void)doSomethingWithString:(NSString *)theString {
	  ...
	}
	
별표 앞에 공백은 선택사항이다. 새로운 코드를 추가할 때는 일관된 형식을 사용한다.

만약 파라미터가 많아서 한 줄에 작성할 수 없다면 하나의 파라미터당 한줄씩 여러 줄로 작성한다. 여러줄로 작성할때는 파라미터 앞에 콜론을 기준으로 정렬한다.

	- (void)doSomethingWith:(GTMFoo *)theFoo
    	               rect:(NSRect)theRect
        	       interval:(float)theInterval {
	  ...
	}

만약 첫번째 줄이 짧아서 정렬할 수 없는 경우에는 두번째 줄에 오는 파라미터는 최소 4개의 공백을 띄우고 작성한다. 그리고 나머지는 두번째 줄에 정렬한다.

	- (void)short:(GTMFoo *)theFoo
          longKeyword:(NSRect)theRect
    evenLongerKeyword:(float)theInterval
                error:(NSError **)theError {
	  ...
	}

#### 메서드 호출

메서드 호출은 메서드 선언 형식과 유사하다. 포멧팅 스타일을 선택할 때는 소스파일에서 이미 사용하고 있는 형식을 따른다.

메서드 호출은 모든 인자는 한줄에 작성한다.

	[myObject doFooWith:arg1 name:arg2 error:arg3];
	
또는 한줄에 한 인자씩 콜론을 기준으로 정렬하여 작성한다.

	[myObject doFooWith:arg1
	               name:arg2
            	  error:arg3];

다음과 같은 형식은 피한다.

	[myObject doFooWith:arg1 name:arg2  // some lines with >1 arg
              error:arg3];

	[myObject doFooWith:arg1
	               name:arg2 error:arg3];
	
	[myObject doFooWith:arg1
	          name:arg2  // aligning keywords instead of colons
	          error:arg3];

메서드 선언 및 정의와 같이 첫번째 인자의 길이가 짧은 경우 두번째 인저는 최소 4개의 공백 이후에 작성하고 나머지 줄은 두번째 줄에 콜론에 맞춘다.

	[myObj short:arg1
          longKeyword:arg2
    evenLongerKeyword:arg3
                error:arg4];

#### @public과 @private

@public, @private는 1개의 공백으로 들여쓰기한다.

	@interface MyClass : NSObject {
	 @public
	  ...
	 @private
	  ...
	}
	@end

#### 예외

예외 구문의 형식은 각 @label의 끝에 공백을 두고 여는 중괄호({)로 끝나도록 한다. 
만약 Objective-C에서 예외 구문을 사용한다면 형식은 다음과 같다. 하지만 예외를 사용하지 말아야할 이유를 보려면 [Avoid Throwing Exception](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml#Avoid_Throwing_Exceptions)을 읽어봐라.

	@try {
	  foo();
	}
	@catch (NSException *ex) {
	  bar(ex);
	}
	@finally {
	  baz();
	}

#### 프로토콜

타입과 프로토콜 꺽쇠 사이에 공백을 두지 않는다. 
이 규칙은 클래스 선언, 인스턴스 변수, 메서드 선언에 마찬가지로 적용된다.

	@interface MyProtocoledClass : NSObject<NSWindowDelegate> {
	 @private
	  id<MyFancyDelegate> _delegate;
	}
	- (void)setDelegate:(id<MyFancyDelegate>)aDelegate;
	@end

#### 블럭

블럭은 콜백 처리등과 같은 타겟 셀렉트 패턴을 위해 제공되는 기능으로 코드를 보다 읽기 쉽게 만들어준다. 블럭 안에 코드는 4개의 공백으로 들여쓰기한다.
블럭은 블럭의 길이에 따라 적용되는 몇가지 규칙이 있다.

* 만약 블럭이 한 줄에 작성 가능하다면 줄바꿈은 사용하지 않는다.
* 만약 줄 바꿈이 필요하다면 닫는 괄호는 블럭이 선언된 첫번째 문자의 라인과 맞춘다.
* 블럭 안에 있는 코드는 4개의 공백으로 들여쓴다.
* 만약 블럭이 20줄을 넘는 정도로 크다면, 로컬 변수로 블럭을 분리하는 것을 추천한다.
* 블럭에 파라미터가 없다면 ^{ 사이에 공백을 쓰지 않는다. 만약 블럭에 파라미터가 있다면 ^(타입과 파라미터간에 공백하나만 허용) {문자 사이에 공백을 쓰지 않는다.
* 블럭 안에 2개의 공백을 사용한 들여쓰기는 프로젝트 코드의 나머지 부분에서도 일관적으로 사용될 때만 허용한다.

코드

	// The entire block fits on one line.
	[operation setCompletionBlock:^{ [self onOperationDone]; }];
	
	// The block can be put on a new line, indented four spaces, with the
	// closing brace aligned with the first character of the line on which
	// block was declared.
	[operation setCompletionBlock:^{
	    [self.delegate newDataAvailable];
	}];
	
	// Using a block with a C API follows the same alignment and spacing
	// rules as with Objective-C.
	dispatch_async(_fileIOQueue, ^{
	    NSString* path = [self sessionFilePath];
	    if (path) {
	      // ...
	    }
	});
	
	// An example where the parameter wraps and the block declaration fits
	// on the same line. Note the spacing of |^(SessionWindow *window) {|
	// compared to |^{| above.
	[[SessionService sharedService]
	    loadWindowWithCompletionBlock:^(SessionWindow *window) {
	        if (window) {
	          [self windowDidLoad:window];
	        } else {
	          [self errorLoadingWindow];
	        }
	    }];
	
	// An example where the parameter wraps and the block declaration does
	// not fit on the same line as the name.
	[[SessionService sharedService]
	    loadWindowWithCompletionBlock:
	        ^(SessionWindow *window) {
	            if (window) {
	              [self windowDidLoad:window];
	            } else {
	              [self errorLoadingWindow];
	            }
	        }];
	
	// Large blocks can be declared out-of-line.
	void (^largeBlock)(void) = ^{
	    // ...
	};
	[_operationQueue addOperationWithBlock:largeBlock];


## 네이밍

네이밍 규칙은 코드를 유지보수 하는데 매우 중요하다. Objective-C 메서드 이름은 긴편이다. 하지만 이것은 코드를 마치 글을 읽듯이 읽을 수 있는 장점이 있고 불필요한 많은 주석도 없앨수 있다.
순수한 Objective-C 코드를 작성할때, 대부분 [Objective-C naming rules](http://developer.apple.com/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)를 따른다. 이 네이밍 규칙은 일반적인 C++ 네이밍 규칙과 조금 다르다. 예를 들어 구글 C++ 네이밍 규칙에는 변수명에서 단어사이에 언더바를 사용하도록 추천한다. 하지만 이 가이드라인에서는 Objective-C 커뮤니티에서 일반적인 단어 사이에 대소문자로 구분하는 intercaps 방식을 추천한다.
어떤 클래스나 카테고리 또는 변수 이름에는 이름안에 대문자로 된 약자를 넣는 경우도 있다. 이것은 URL, TIFF 또는 EXIF와 같은 대문자 약자를 이름에 포함하는 애플 표준을 따라서이다.
하지만 Objectivie-C++ 코드를 작성할 때는 이런 규칙에 너무 엃매이지 않는다. 많은 프로젝트들은 Objective-C나 코코아 또는 C++ 백엔드와 코코아 프론트엔드를 연결하는 브릿지등에서 크로스 플랫폼 C++ API 구현을 필요로한다. 이런 상활에서는 상반되는 두개의 코딩 가이드가 필요한 경우가 생긴다.
이 문제를 해결하기 위해서 @implemetation 안에서는 Objective-C 코딩 규칙을 따르고 C++ 클래스 안에서는 C++ 네이밍 규칙을 따른다. 하나의 메서드에서 두개의 규칙을 섞어쓰는 것은 코드의 가독성을 해치므로 피해야한다.

#### 파일명

파일명은 클래스 구현의 이름과 대소문자까지 같아야한다. 확장자는 다음과 같다.

확장자| 설명
----|-------------------------
.h	| C/C++/Objective-C 헤더파일 
.m	| Objective-C 구현 파일 
.mm	| Objective-C++ 구현 파일
.cc	| 순수 C++ 구현 파일
.c	| C 구현 파일


카테고리 이름은 클래스이름을 확장해서 사용해야한다. 예) GTMNSString+Utils.h or GTMNSTextView+Autocomplete.h

#### Objective-C++

코코아/Objective-C와 C++이 함께 사용되는 경우 충돌을 최소화하기 위해 메서드가 구현된 곳의 스타일 규칙을 따른다. 만약 @implementation 안에서 구현된것은 Objective-C 네이밍 규칙을 따르고 C++ 클래스에서 구현된 것은 C++ 네이밍 규칙을 따른다.

	// file: cross_platform_header.h

	class CrossPlatformAPI {
	 public:
	  ...
	  int DoSomethingPlatformSpecific();  // impl on each platform
	 private:
	  int an_instance_var_;
	};
	
	// file: mac_implementation.mm
	#include "cross_platform_header.h"
	
	// A typical Objective-C class, using Objective-C naming.
	@interface MyDelegate : NSObject {
	 @private
	  int _instanceVar;
	  CrossPlatformAPI* _backEndObject;
	}
	- (void)respondToSomething:(id)something;
	@end
	@implementation MyDelegate
	- (void)respondToSomething:(id)something {
	  // bridge from Cocoa through our C++ backend
	  _instanceVar = _backEndObject->DoSomethingPlatformSpecific();
	  NSString* tempString = [NSString stringWithInt:_instanceVar];
	  NSLog(@"%@", tempString);
	}
	@end
	
	// The platform-specific implementation of the C++ class, using
	// C++ naming.
	int CrossPlatformAPI::DoSomethingPlatformSpecific() {
	  NSString* temp_string = [NSString stringWithInt:an_instance_var_];
	  NSLog(@"%@", temp_string);
	  return [temp_string intValue];
	}

#### 클래스명

클래스명(카테고리명, 프로토콜명 포함)은 대문자로 시작하며 단어의 구분은 다음 단어가 시작되는 첫글자를 대문자로 표기해서 사용한다.
어플리케이션 레벨의 코드에서 클래스명앞에 Prefix는 일반적으로 피하는 것이 좋다. 모든 클래스가 같은 Prefix 가지는 것은 아무 이득이 없고 가독성을 해치기만한다. 만약 코드가 다른 어플리케이션과 공유되어야 하는 목적으로 설계되었다면 Prefix는 허용되고 추천한다. (예 GTMSendMessage)

#### 카테고리명

i

## 주석
## 코코아와 Objective-C 기능
## 코코아 패턴
