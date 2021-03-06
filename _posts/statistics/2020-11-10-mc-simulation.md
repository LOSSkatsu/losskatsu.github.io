---
title: "몬테카를로 시뮬레이션(monte carlo simulation) 기초" 
categories:
  - statistics
tags:
  - statistics
use_math: true
toc: true
toc_label: "My Table of Contents"
toc_icon: "cog"
sidebar:
  title: "AI Machine Learning"
  nav: sidebar-contents
---


# 몬테카를로 시뮬레이션(monte carlo simulation) 기초 

본 포스팅은 [MIT 강의](https://www.youtube.com/watch?v=OgO1gpXSUzU&ab_channel=MITOpenCourseWare)를 참고했습니다. 

**참고 자료**

* [모집단, 표본 개념 복습하기](https://losskatsu.github.io/statistics/population-sample/)
* [분산 개념 복습하기](https://losskatsu.github.io/statistics/mean-vairance/)

## 1. 몬테카를로 시뮬레이션의 역사

한 수학자가 살고 있었습니다. 
그는 몸이 아팠을 때 도박을 했었는데, 자신이 카드게임에서 이길 확률을 추정하고 싶었습니다. 
그래서 몸소 카드게임을 무한 반복 하면서 확률을 추정했습니다. 
자신이 직접 경험 하면서요. 
근데 본인이 계속 카드게임을 너무 힘든 겁니다. 
그래서 내가 직접 게임을 계속하기보다는 컴퓨터로 시뮬레이션을 해보자라는 생각을 하게 됩니다. 
그런데 그는 컴퓨터를 잘 몰랐기에 친구였던 폰 노이만에게 에니악으로 시뮬레이션을 돌려달라고 합니다. 
이것이 몬테카를로 시뮬레이션(Monte Carlo Simulation)의 탄생 이야기 입니다. 
몬테카를로 시뮬레이션은 카드 게임 뿐만이 아니라 수소폭탄 설계에도 쓰입니다. 

## 2. 몬테카를로 시뮬레이션의 정의

몬테카를로 시뮬레이션이란 알려지지 않은 값을 추론적 통계(inferential statistics)방법을 이용해 추정하는 것을 의미합니다. 
추론적 통계에서 중요한 개념은 모집단(population)과 샘플(sample)입니다. 

### 2.1. 모집단 

모집단(population)은 가능한 예의 전체 우주, 세계를 의미합니다. 
카드게임을 예로 들면 플레이 가능한 모든 상황을 의미합니다. 카드게임이니 엄청나게 많은 상황이겠네요. 
모집단에 대한 자세한 내용은 [모집단 링크](https://losskatsu.github.io/statistics/population-sample/)를 확인해주세요.


### 2.2. 샘플

[샘플(sample)](https://losskatsu.github.io/statistics/population-sample/)은 모집단의 부분집합(subset)입니다. 샘플 수는 적어도 0보다는 커야합니다. 
그리고나서 샘플을 통해 모집단에 대한 추론을 이끌어내는 것입니다. 
모집단은 굉장히 큰 집합인 반면 샘플은 상대적으로 더 작은 집합이죠. 
이게 가능하게 하려면 샘플을 잘 선택하는게 중요한데, 만약 우리가 랜덤으로 샘플을 선택한다면 
이 샘플은 모집단과 비슷한 특성을 보입니다. 
여기서 가장 중요한 요인(key fact)은 랜덤 샘플(random sample)이라는 것입니다. 랜덤. 
만약 여러분이 샘플을 랜덤으로 뽑지 않는다면, 해당 샘플이 모집단과 동일한 특성을 가질것이라고는 기대하기 힘듭니다. 

### 2.3. 랜덤 샘플의 중요성(예)

랜덤 샘플이 얼마나 중요한 지 간단히 동전을 던지는 예를 들어 알아봅시다. 
만약 동전을 던졌는데 앞면이 나왔다고 하면, 
앞으로 같은 동전을 무한대로 던졌을때도 모두 앞면이 나올 확률이 얼마나 될까요? 
무한대가 아니라 앞으로 한번만 더 던진다고 했을 때 앞면이 나올거라고 얼마나 확신할 수 있나요? 
아마 확신하기는 힘들겁니다. 
이번에는 동전을 두번 던졌을 때 모두 앞면이 나온다고 가정해봅시다. 

<center><img src="/assets/images/statistics/montecarlo-basic/montecarlo.jpg" width="800"></center>

이번에도 같은 질문으로 다음 동전을던졌을때 앞면이 나올거라고 생각하시나요? 
아마 이전에 한번 던졌을때보다는 조금더 확신의 정도가 강해졌을 겁니다. 
하지만 그렇다고해서 확신할수는 없죠. 
그럼 이건 어떤가요? 
동전을 100번 던졌는데, 모두 앞면이 나왔다고 하겠습니다. 
이 경우 동전의 양면이 모두 앞면이라고 의심할 것입니다. 
그리고 다음 동전을 던졌을 때를 예측하라고하면 아마 앞면이라고 예측하겠죠. 
이번엔 조금 다른 예를 들어봅시다. 
동전을 100번 던졌는데, 앞면이 52번 나오고 뒷면이 48번 나왔다고 합시다. 
그렇다면 여러분은 동전을 던졌을 때 앞면이 나올 확률이 52/100이라고 생각하나요? 
아마 주어진 데이터를 이용하면 52/100이 베스트 추정치일 것입니다. 
명확한 증거를 기반으로 했을 때, 52/100이 추정 확률로는 베스트 인것이죠. 

<center><img src="/assets/images/statistics/montecarlo-basic/montecarlo02.jpg" width="800"></center>

하지만 이런 추측을 아주 강하게 확신하기는 힘듭니다. 
왜냐면 우연히 발생했을 수도 있으니까요. 
그렇다면 샘플수가 100이었을 때는 어째서 샘플수가 2였을 때보다 더 나은 추측을 할 수 있는 것일까요? 
답은 분산에 있습니다.

### 2.4. 분산

동전을 100번 던졌을 때 100번 모두 앞면이 나왔을 때는 [분산](https://losskatsu.github.io/statistics/mean-vairance/)이 없었습니다. 답이 항상 같았으니까요. 
분산이 적다는 것은 정답에 대한 확신을 강하게 만들어줍니다. 즉, 샘플을 통해 얻은 답이 모집단의 특성을 가리킨다는 것에 대한 확신이죠. 
반면 100번 던졌는데 앞면, 뒷면이 반반씩 나온건 분산(variance)이 커졌음을 의미합니다. 
다음번 동전을 던졌을 때 예측하는게 훨씬 어려워졌다는 말이죠. 
따라서 분산이 커질수록 같은 크기의 확신을 얻기위해 더 큰 샘플이 필요하다는 의미입니다. 

### 2.5. 룰렛 게임의 예 

이번에는 룰렛을 예로 들어봅시다. 룰렛은 공이 멈추는 숫자와 색깔을 맞추는 게임입니다. 
룰렛 게임은 다음과 같이 프로그래밍 할 수 있습니다. 

```python
import random

class FairRoulette():
    def __init__(self):
        self.pockets = []
        for i in range(1, 37):
            self.pockets.append(i)
        self.ball = None
        self.pocketOdds = len(self.pockets) - 1
    def spin(self):
        self.ball = random.choice(self.pockets)
    def betPocket(self, pocket, amt):
        if str(pocket) == str(self.ball):
            return amt*self.pocketOdds
        else: return -amt
    def __str__(self):
        return 'Fair Roulette'
```

자 이제 게임을 시작해봅시다. 

```python
def playRoulette(game, numSpins, pocket, bet, toPrint):
    totPocket = 0
    for i in range(numSpins):
        game.spin()
        totPocket += game.betPocket(pocket, bet)
    if toPrint:
        print(numSpins, 'spins of', game)
        print('Expected return betting', pocket, '=', str(100*totPocket/numSpins) + '%\n')
    return (totPocket/numSpins)
    
game = FairRoulette()
for numSpins in (100, 100000):
    for i in range(3):
        playRoulette(game, numSpins, 2, 1, True)
```

위 실험의 결과는 다음과 같습니다. 

```
100 spins of Fair Roulette
Expected return betting 2 = 44.0%

100 spins of Fair Roulette
Expected return betting 2 = 8.0%

100 spins of Fair Roulette
Expected return betting 2 = -28.0%

100000 spins of Fair Roulette
Expected return betting 2 = -0.712%

100000 spins of Fair Roulette
Expected return betting 2 = 3.968%

100000 spins of Fair Roulette
Expected return betting 2 = -0.928%
```

100번을 던졌을 때는 변동성이 큽니다. 이게 도박의 매력이죠. 
100번했는데 승률이 44%면 꽤 이길 가능성이 높다고 생각합니다. 
100000번 했을 때는 분산이 훨씬 작아집니다. 
100번 던졌을 때와 비교하면 결과가 -0.7, 3.9, -0.9로 거의 0에 가까워지네요.

## 3. 대수의 법칙

위에서 플레이한 룰렛 게임으로 알 수 있는 것이 대수의 법칙(law of large number)입니다. 
대수의 법칙은 통계학에서 가장 중요한 개념입니다. 
대수의 법칙이란 성공확률이 p인 독립시행을 무한히 반복한다면 
사건 발생확률이 p가 아닐 확률이 0에 수렴한다는 법칙입니다. 
즉, 위 룰렛의 예에서 룰렛을 무한히 반복한다면 기대 수익은 0이라는 의미입니다. 
중요한 것은 '무한히'라는 표현인데, 따라서 100번 보다는 100만번이 더 좋은 방법이라는 뜻이 됩니다. 
가끔 이 법칙을 잘못이해하는 사람들이 있는데, 이를 gambler's fallacy라고 합니다. 
야구를 보는데 평소 타율이 높았던 타자가 헛스윙을 여러번했다고합시다. 
이때 아나운서는 다음번 타석에서 저 타자가 잘 칠것이라고 말합니다. 
왜냐면 그는 평소에 잘 치기 때문이죠. 
즉, 사람들은 편차가 미래로 갈수록 잠잠해질 것이라고 생각하는 것입니다. 
하지만 이는 사실이 아닙니다. 
룰렛 이야기를 다시 해봅시다. 
만약 여러분이 검정이나 빨강에 배팅한다고 합시다. 
26번 연속으로 빨강이 나올 확률은 1/67108865 입니다.  
그런데 첫번째 시행부터 25번째 시행까지 연속으로 빨강이 나왔다고 했을 때, 
26번째가 빨강일 확률은 1/2 입니다. 왜냐면 이는 독립시행이기 때문입니다. 
사람들이 gambler's fallacy와 헷갈리는 것은 평균으로의 회귀입니다.

## 4. 평균으로의 회귀

평균으로의 회귀(regression to the mean)는 이름 그대로 평균으로 회귀한다는 의미입니다. 
아버지의 키가 아주 큰 경우에는 아들의 키가 아버지 키보다 클 확률은 적다는 것이죠. 
룰렛을 예로 들면, 공정한 룰렛을 10번 돌려서 10번 빨강이 나왔다고 하면, 이는 극단적인 상황입니다. 
1/1024의 확률이 발생한 것이죠. 
이때 gambler's fallacy를 적용하면, 이후 10번 더 돌리면 검정이 많이 나올 것이라고 생각하는 것입니다. 
그러나 평균으로의 회귀 개념은 이와 다릅니다. 
이후 10번을 더 돌렸을 때 10번의 빨강보다는 더 적은 빨강이 나올 것입니다. 
즉, 이전보다는 덜 극단적인 상황이 나올 것이라는 것이죠. 
따라서 빨강이 10번보다는 덜 나올 것이라는 것 입니다. 
따라서 총 20번의 평균을 내면 빨강이 나올 확률은 50%에 더 가까워지겠죠. 
이것이 평균으로의 회귀이고, 샘플이 커질수록 평균에 가까워진다는 것입니다. 
즉, gambler's fallacy는 이전에 빨강이 10번 나왔으니 다음 10번의 시행에서는 빨강이 5번 미만으로 나올 것이라고 예측하는 것입니다. 
반면 평균으로의 회귀는 다음 10번의 시행에서 빨강이 10번보다는 적게 나올 것이라고 예측하는 것입니다. 

## 5. 샘플의 분산, 표준편차

여러분이 샘플링을 할 때 완벽한 정확도를 얻는 것을 보장할 수는 없습니다. 
이상한 샘플을 얻을 수도 있다는 것이죠. 
여러분이 정확한 답을 얻지 못할 것이라고 말하는 것이 아닙니다. 
지금부터 분산과 표준편차에 대해 이야기해보겠습니다. 
표준편차는 항상 평균과 비교해서 생각해야합니다. 
예를 들어 표준편차가 100이라고 하면 이것은 큰 값일까요 작은 값일까요?
답은 알 수 없습니다. 
하지만 만약 평균에 관한 정보가 주어져서 평균이 100이고 표준편차가 100이라면 
이는 엄청 큰 표준편차를 의미합니다. 
만약 평균이 1백만이고, 표준편차가 100이라면 이때 표준편차는 아주 작은것이죠. 

## 6. 신뢰구간 

자, 여기서 오늘의 하이라이트가 나옵니다. 
우리는 종종 평균만 이용해서 추정합니다. 예를 들어 이번 시험 평균점수가 80점이다라는 식으로 말이죠. 
하지만 우리가 모집단의 특성을 추정할 때 하나의 값으로 추정하는 것 보다는 신뢰구간을 이용해서 추정하는 것이 더 좋습니다. 
바로 이 신뢰구간을 추정할 때 표준편차의 개념이 사용됩니다. 
우리는 평소에 평균 따로, 표준편차 따로 구해서 둘이 연결되어 있다는 생각은 잘 못하는데, 
신뢰구간이라는 개념에서 평균과 표준편차가 함께 사용되는 것입니다. 
