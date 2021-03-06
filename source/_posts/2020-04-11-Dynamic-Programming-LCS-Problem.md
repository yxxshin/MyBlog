---
layout: post
title: "Dynamic Programming - LCS problem"
description: Dynamic Programming을 이용한 LCS problem
date:   2020-04-11 18:00:00 +0900
categories: [ Algorithm, Theory ]
tags: [ dp ]
---

우선, Dynamic Programming에 대해 알아보자.

 **Dynamic Programming (동적계획법)** 이란, 쉽게 말해 큰 문제를 작은 문제로 나누어 푸는 것을 말한다. 여기까지는 분할정복 (Divide and Conquer)와 개념이 일치한다. Dynamic Programming의 핵심 포인트는, **작은 문제들을 반복해서 풀지 않는다는 것**이다. 이는 시간을 크게 절약할 수 있다. 
<!-- more -->
 Dynamic Programming은 작은 문제들이 반복되서 나오고, 이들의 답이 바뀌지 않을 때 유용하게 사용된다. 필자가 현재까지 Baekjoon에서 동적계획법 문제를 풀면서 가장 지겹도록 풀었던 문제 유형은 '점화식' 유형들이다.

 피보나치 수열을 예로 들어보자. 우리는 앞에서 재귀호출을 이용하여 피보나치 수열의 값을 구한 바 있다. 하지만, 재귀호출을 이용하면 우리는 이미 3번째, 4번째 피보나치 수를 계산하기 위해 다시 재귀 구문을 돌려야 한다. 이는 시간 낭비이다. 피보나치 수를 담아놓는 배열을 만들어, 구한 값을 이 배열에 담아놓자. 배열에 담긴 수는 이미 이전에 구한 수이므로, 또 다시 계산할 필요 없이 가져다 쓰면 된다. 

 위와 같은 점화식 문제들은 점화식만 잘 세우면 쉽게 코딩할 수 있기 때문에, 수학 문제라고 부를 수 있다. 이 블로그에서 이번에 Dynamic Programming에 대해 다루려는 문제는 크게 두 개인데, 바로 LCS problem과 knapsack problem 이다. 이들은 특유의 Dynamic Programming 알고리즘을 모르면 매우 고생하게 된다.

 **LCS(Longest Common Subsequence, 최장 공통 부분 수열) 문제**는 두 수열이 주어지면, 모두의 부분 수열이 되는 수열 중 가장 긴 것을 찾는 문제이다. 예를 들어, ACAYKP와 CAPCAK의 LCS는 ACAK이다. 이것과 구분해야 되는 것은 LCS(Longest Common Substring)이다. Subsequence는 부분 수열이고, Substring은 문자열이다. Subsequence는 연속하지 않아도 되며, Substring은 연속해야 한다는 차이가 있다.

LCS 알고리즘은 두 수열을 2차원 표에 적으면서 시작된다. 

![LCS-1](https://imgur.com/1kQoOSl.png)

위 예시는 두 수열 ACAYKP, CAPCAK의 LCS를 구하는 예시이다. 우선, 첫 줄은 모두 0을 적는다. 이후, 나머지 표의 칸은 다음의 규칙에 맞추어 채운다. 밑에서 말하는 두 문자는, 그 칸에 대응되는 각 수열의 문자를 의미한다.

1. 두 문자가 같으면, 왼쪽 위 대각선의 값에 1을 더하여 적는다.
2. 두 문자가 다르면, 윗 칸과 왼쪽 칸을 비교하여 더 큰 값을 적는다.

위와 같이 표를 완성하였다면, 구하고자 하는 LCS의 길이는 가장 오른쪽 아래의 값이다! 표를 완성시킨 규칙을 잘 생각해 보면, 왜 위와 같은 규칙으로 표를 채워 나가는지, 그리고 왜 마지막 값이 LCS의 길이가 되는지 알 수 있을 것이다. 

이제, LCS가 되는 수열(문자열)을 구해보자. 위 표에서 구해낼 수 있다. 
위 표를 보게 되면, 각 숫자의 '영역'이 있음을 확인할 수 있다. 0의 영역, 1의 영역, 2의 영역, ... 이 영역을 잘 유의하면서 아래 규칙을 따른다. 가장 오른쪽 아래 칸에서 시작한다.

![LCS-2](https://imgur.com/g89goUo.png)

1. 왼쪽 칸, 혹은 위쪽 칸으로 이동할 수 있다. 단, 숫자가 같아야 한다. (같은 영역의 칸이어야 한다.)
2. 더 이상 이동할 수 없다면, 왼쪽 위 대각선으로 한 칸 이동한다. 이 때, 이동하기 전 칸의 문자를 기록한다. 
3. 0의 영역에 도달할 때까지 위 작업을 반복하고, 기록한 문자들을 역으로 출력해낸다.


참고: 2번의 과정에서 기록되는 칸은 두 수열에서 (당연히) 같은 문자를 가르키는 칸이어야 한다! 

필자는 LCS의 개수를 구하는 코드는 짰으며(Baekjoon 9251), [여기][code]에서 확인할 수 있다.


[사진출처][pic]

[pic]: https://hooit.tistory.com/entry/LCSLongest-Common-Subsequence-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-C%EC%96%B8%EC%96%B4
[code]: https://yxxshin.github.io/2020/04/18/2020-04-18-Baekjoon-9251/
