---
layout: post
title: "컴퓨터와 수학을 공부해야 할까? 4편"
description: "튜링 머신과 컴퓨터의 시뮬레이션"
date: 2019-08-16
tags: [Computer science, Computer Engineering, Mathematics, 컴퓨터, 컴퓨터과학, 컴퓨터공학, 수학]
category: "Why computer?"
img: TMSmartphone.png
comments: true
share: true
---

*[3편](http://hjeon.ml/2019-08-13/why-computer-and-math-2/)을 읽고 와 주세요~*


지난 글에서 나는 튜링 머신과 이 개념이 제시된 논문에 대해 잠시 소개했다. 대충 요약하자면 튜링 머신이라는 개념을 만든 튜링은 힐베르트가 제시한 문제를 증명하는 데 성공했고 어쩌구 하는 내용이었는데, 이 이야기는 잠시 미뤄두고 일단 튜링 머신이 무엇인지에 대해 알아보자. 

## 튜링 머신의 구조

튜링은 1948년 에세이 "지능형 기계"에서 튜링 머신에 대해 다음과 같이 설명했다.

> *...an unlimited memory capacity obtained in the form of an infinite tape marked out into squares, on each of which a symbol could be printed. At any moment there is one symbol in the machine; it is called the scanned symbol. The machine can alter the scanned symbol, and its behavior is in part determined by that symbol, but the symbols on the tape elsewhere do not affect the behavior of the machine. However, the tape can be moved back and forth through the machine, this being one of the elementary operations of the machine. Any symbol on the tape may therefore eventually have an innings.*

> *이 기계의 제한 없는 저장 용량은 문자(symbol)을 적을 수 있는 사각형 공간들로 구성된 무한한 테이프로 구현된다. 매 순간 기계는 한 개의 문자를 읽을 수 있고, 이를 "읽고 있는 문자"라 부르자. 기계는 읽고 있는 문자를 변경할 수 있으며, 기계의 행동은 그 문자에 의해 부분적으로 결정된다. 그러나 그 문자를 제외한 테이프의 다른 문자들은 기계의 행동에 영향을 미칠 수 없다. 그러나 테이프는 기계를 통해 앞뒤로 움직일 수 있고, 이것은 기계의 기본 조작 중 하나다. 그러므로 테이프 위의 모든 문자는 언제든지 기계에 의해 읽힐 가능성이 있다.*

<span style="color:gray">~~내 능력으로 더 나은 번역은 힘든 듯...매번 생각하지만 번역가들은 정말 대단한 것 같다~~</span>

이 설명이 사실 튜링 머신의 전부지만, 어떻게 작동하는지는 이 설명만 가지고 알기 힘들다. 그림을 보면 좀 더 이해가 쉬울 것이다. 

<center><img src="https://jeonhyun97.github.io/images/TMmyPic.png" width="450" ></center>  


생각보다 단순하다는 생각이 들 것이다. 그림을 보면 튜링 머신은 크게 4가지 요소로 구성됨을 알 수 있는데, 각 요소에 대한 설명은 다음과 같다. 

- 테이프(Infinite Tape) <br>
튜링이 말한 "사각형 공간들로 구성된 무한한 테아프"다. 튜링 머신이 어떠한 동작을 하기 위해 필요한 입력값들은 이 테이프로부터 읽을 수 있으며, 어떠한 동작의 결과로 나오는 출력값도 이 테이프 위에 써서 나타낸다. 
- 헤드(head) <br>
위 그림에서 화살표가 바로 헤드다. 튜링의 설명에서 우리는 매 순간 기계는 한 개의 문자를 읽을 수 있고, 기계의 행동은 그 문자에 의해 부분적으로 결정된다는 말을 찾을 수 있다. 튜링 머신에서 헤드는 기계가 현재 "읽고 있는 문자"를 가리키는 역할을 한다. 
- 상태 기록소(Finite State Register) <br>
어쩄든 튜링 머신이 무엇인가 기능을 하기 위해서는 현재 무슨 기능을 수행하고 있는지, 앞으로 무엇을 해야 할지 등등을 기억해야 한다. 쉽게 말해, 덧셈 연산을 하다가 갑자기 자기가 덧셈을 한다는 것을 까먹고 곱셈을 해버리면 안되니까 "나는 지금 덧셈을 하고 있다" 라고 기억하게 해주는 장치인 것이다. 여기서 테이프와 다르게 상태 기록소는 유한해야 한다는 점을 기억해 두자.
- 명령표(Instruction table) <br>
현재 헤드가 가리키고 있는 "읽고 있는 문자"와 상태 기록소가 알려주는 "현재 상태"에 대한 정보를 바탕으로 이 다음에 무슨 동작을 해야 하는지 알려준다. 예를 들어, 현재 헤드가 가리키고 있는 문자가 A이고 지금 상태가 B상태라면 헤드가 가리키는 칸의 문자를 C로 바꾸고 상태 D로 이동하라! 따위의 명령을 적어놓는 요소다. 튜링의 설명에는 "기계의 행동은 현재 읽고 있는 문자에 의해 부분적으로 결정된다"라는 말이 있는데, 바로 이 내용을 의미한다. 튜링 머신은 매 순간 다음과 같은 명령들을 수행할 수 있다. <br>
    1. 현재 헤드가 가리키고 있는 칸의 문자를 지우거나 새로 쓰기
    2. 헤드를 앞쪽 혹은 뒤쪽으로(위 그림에서는 왼쪽 혹은 오른쪽으로) 한 칸 이동하거나 가만히 놔둔다.
    3. 튜링 머신을 새로운 상태로 이동시킨다. 다시 말해, 상태 기록소의 정보를 변경한다. <br>

천천히 읽어보면 튜링의 설명이 튜링 머신이 할 수 있는 모든 기능을 명확하게 표현했음을 알 수 있다 (물론 내 번역은 그렇지 못하다. 영잘알이라면 원문 읽어주세요 ㅜㅜ). 그런데, 나는 지난 편 말미에서 현재 우리가 사용하는 모든 컴퓨터가 튜링 머신을 기초로 만들어졌다는 말을 했다. 좀 더 정확하게 표현하자면, 이 세상 모든 컴퓨터와 튜링 머신은 동등한 컴퓨팅 능력을 가진다. 예를 들어, 지금 이 글을 읽고 있는 당신의 스마트폰(혹은 컴퓨터)에 대해 생각해 보자. 당신의 스마트폰이 할 수 있는 모든 일을 튜링 머신으로 할 수 있고, 저장 용량만 무한하다면 튜링 머신이 할 수 있는 모든 일을 스마트폰으로도 할 수 있다.

## 튜링 머신 == 스마트폰??

이게 언뜻 보면 말도 안 된다고 생각할 수 있다. 일단 딱 봐도 튜링 머신으로 유튜브를 보거나 히오스를 하는 건 불가능해 보인다. 하지만 조금만 생각해 보면 튜링이 설명한 내용이 컴퓨터가, 그리고 당신의 스마트폰이 지금 하고 있는 행동의 전부다.

<center><img src="https://jeonhyun97.github.io/images/WhatDogSound.png" width="350" ></center>  
 

*<center>이게 먼 개소리야!</center>*

스마트폰으로 유튜브를 보는 행위를 예로 들어 보자. 유튜브에 올라온 동영상은 세계 어딘가에 있는 구글 서버에 저장되어 있을 텐데, 우리가 보고 싶은 동영상을 선택하면 스마트폰은 먼저 서버에서 동영상의 정보를 복사한다. 그 후 이 정보를 스마트폰의 화면을 통해 출력한다. <br>
이 과정을 튜링 머신으로 시뮬레이팅해 보자. 일단 먼저 스마트폰을 "컴퓨터"라 불릴 수 있게 하는 중요한 요소가 무엇인지 알아볼 필요가 있다. 유튜브 영상을 직접 보여주는 "터치스크린". 당연히 정답이 아니다. 손가락 터치는 마우스나 키보드 등 다른 입력 장치로 대체할 수 있고, 무언가를 보여주는 화면 역시 컴퓨터에게 필수적인 요소는 아니다. 모니터가 없어도 컴퓨터를 쓸 수는 있지 않은가. 컴퓨터가 보여주는 출력은 소리와 같은 다른 방식으로 충분히 표현될 수 있다. 만일 아니라면 시작장애인이 컴퓨터나 스마트폰을 사용하는 것은 불가능할 것이다. 같은 이유로 스피커, 진동 발생 장치, 카메라 등등 스마트폰에 주렁주렁 달려있는 입출력 장치들은 정답이 될 수 없다. 그렇다면 대체 어떤 요소가 컴퓨터로써의 정체성을 확립하는 것일까.

정답은 직접 계산(Computing)을 수행하는 중앙처리장치(CPU), 그리고 계산과 관련된 여러 가지 정보를 저장하는 저장 장치, 즉 메모리다. 이 둘 중 하나라도 없다면 컴퓨터, 아니 스마트폰은 1+1과 같은 간단한 연산조차 할 수 없다. 뭐 CPU의 필요성은 자명하다. CPU가 없으면 컴퓨터는 1+1이라는 명령을 읽어 봤자 1이 뭐고 + 기호가 뭔지도 모를 것이다. 애초에 무엇인가 정보를 읽어들일 수도 없는, 뇌가 없는 상태라고 볼 수 있다. 그렇다면 메모리는? 메모리가 없다면 1+1이라는 명령을 읽는 과정에서 두번째 1을 읽어들일 때 첫번째 숫자가 1이었는지 2였는지 3.141592였는지 알 방법이 없다. 때문에 역시 아무런 연산을 하지 못한다. 

그렇다면 스마트폰이 외부와 소통할 수 있게 해주는 네트워크 장치는 어떨까? 역시 필수적인 요소는 아니다. 스마트폰을 비행기 모드로 설정해도 이미 저장되어 있는 오프라인 컨텐츠를 소비하거나, 인터넷 연결이 필요없는 앱을 실행하는 데는 아무런 문제가 없다. 네트워크 장치는 한정된 용량을 가지는 스마트폰의 저장 장치를 다른 장치와의 연결을 통해 확장하는 역할을 한다고 볼 수 있다. 

자, 그러면 이제 스마트폰의 CPU와 메모리를 튜링 머신으로 바꾸고, 거기다가 터치크스린이나 스피커 등 각종 입출력 장치를 주렁주렁 달아서 유튜브 동영상을 틀어보도록 하자.

<center><img src="https://jeonhyun97.github.io/images/TMSmartphone.png" width="500" ></center>  


CPU를 상태 기록기, 메모리를 테이프로 치환했다. 또한, 위에서도 말했듯이 네트워크를 통해 서버에 저장된 데이터에 접근 가능하다는 것은 곧 저장 공간의 확장을 의미하므로 테이프의 일부분은 서버 데이터를 담고 있다고 표현했다. 위 그림에서 서버에 저장된 유튜브 동영상들은 VA, VB, VC ... 와 같은 이름들을 갖고 있고, 각 이름 옆에 동영상 데이터가 이진수 형식(0과 1로만 이루어짐)으로 저장되어 있다. 그림에 등장하는 X 문자의 경우 각 데이터 파일의 오른쪽 끝에 위치해 파일의 종료를 알린다. 예를 들어, VA 1 0 1 1 X 는 VA라는 이름을 가진 동영상의 데이터가 1011로 저장되어 있음을 의미한다. 

테이프에서 스마트폰에 할당된 부분에는 사용자가 보기로 선택한 동영상의 이름이 저장되고 (Select YouTube Video 부분), 재생하고 싶은 동영상의 정보를 문자 S 옆에 저장할 수 있다. 스마트폰의 화면과 스피커는 테이프를 모니터링하고 있다가 영상을 재생하라는 명령이 들어오면 S부터 시작해 데이터의 끝을 의미하는 X가 나오기 전까지 차례로 데이터를 처리해 사용자에게 보여준다. 이제 준비는 마쳤으니, 우리가 디자인한 튜링 머신이 어떻게 동작하는지 살펴보자.

우리의 "유튜브 동영상 재생 튜링 머신"은 헤드가 선택한 동영상의 이름을 저장한 칸을 가리킨 상태로 시작한다. 시작과 동시에 동영상의 이름을 읽은 튜링 머신은 바로 "동영상 탐색" 상태로 전환되고, 테이프의 왼쪽으로 헤드를 이동하면서 선택한 동영상의 이름 (위 그림에서는 VE) 가 있는지 탐색한다. 동영상은 서버 어딘가에 저장되어 있기 때문에 왼쪽으로 가다 보면 언젠가는 같은 이름을 가진 동영상을 찾을 수 있다. 동영상을 찾은 후에는 "동영상 복사" 상태로 전환해 데이터 정보를 S 문자 옆에 위치하도록 하나하나씩 복사해 오면 된다. 위 그림을 예로 들자면 다음과 같은 과정을 따를 것이다. 

1.  헤드가 VE 바로 옆에 위치한 문자 (그림에서는 1)를 가리키게 하고, 그 값을 복사한다.
2. S를 찾을 때까지 헤드를 오른쪽으로 이동시킨다.
3. 찾은 S 옆 칸에 복사한 문자를 입력한다. 
4. 다시 헤드를 왼쪽으로 이동시켜 VE 데이터의 두번째 문자 (1) 를 찾아 이를 복사한다.
5. 헤드를 S의 오른쪽 두번째 칸으로 이동시켜 이를 입력한다.
6. 같은 방법으로 VE 동영상의 데이터가 끝날 때까지 (X 문자가 나올 때까지) 데이터를 복사해서 S 문자의 오른쪽으로 이동시킨다. 

이 과정이 끝났다면 VE 동영상의 모든 데이터가 S 문자 오른쪽에 성공적으로 복사되었을 것이다. 이후 튜링 머신을 "동영상 재생" 상태로 바꾸고, S 문자 근처를 모니터링하던 스마트폰 화면과 스피커가 동영상 데이터를 처리해 재생하도록 만들면 된다. 이로써 우리는 "유튜브 동영상 재생 튜링 머신"을 성공적으로 구현했다. 

#

사실 이 예시는 굉장히 축약, 간략화된 설명이다. 그럼에도 불구하고 튜링 머신이 컴퓨터가 하는 행동을 시뮬레이팅할 수 있음을 위 설명을 통해 좀 더 쉽게 이해할 수 있었을 것이다. 이외에도, 튜링 머신을 사용하면 덧셈, 뺄셈, 곱셈 등 컴퓨터 CPU가 수행하는 기본적인 연산이나, 어떠한 집합에서 원하는 내용을 검색하는 알고리즘 역시 쉽게 구현할 수 있다 (구글이 알려줄 것이다). 

이제 튜링 머신을 사용해 컴퓨터가 하는 행동을 구현할 수 있다는 것을 알았다. 그러나 아직 부족하다. 위에서 나는 "컴퓨터가 할 수 있는 모든 일은 튜링 머신으로도 할 수 있다" 라고 주장했다. 그런데, 현재 우리는 컴퓨터가 하는 모든 행동에 대해 각각을 수행하는 개별적인 튜링 머신을 만들 수 있다는 것만 알고 있다. 다시 말해, 유튜브 영상을 재생하는 튜링 머신, 덧셈을 하는 튜링 머신, 검색 알고리즘을 수행하는 튜링 머신, 이런식으로 각각의 행동을 수행하는 독립적인 튜링 머신을 만들 수 있다는 것만 보였다. 여기서 하나의 의문이 생긴다- 단 한 대의 튜링 머신으로 컴퓨터의 모든 행동들을 수행할 수 있게 할 수 없을까. 이에 대한 답이 바로 **보편 튜링 머신(Universal Turing Machine)** 이다. 다음 편에서 이 놀라운 기계에 대해 알아보도록 하자.  







