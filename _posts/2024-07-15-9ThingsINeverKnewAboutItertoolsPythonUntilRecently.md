---
title: "알고 나서 놀란 Itertools Python의 9가지 비밀"
description: ""
coverImage: "/TIL/assets/img/2024-07-15-9ThingsINeverKnewAboutItertoolsPythonUntilRecently_0.png"
date: 2024-07-15 00:04
ogImage:
  url: /TIL/assets/img/2024-07-15-9ThingsINeverKnewAboutItertoolsPythonUntilRecently_0.png
tag: Tech
originalTitle: "9 Things I Never Knew About Itertools Python Until Recently"
link: "https://medium.com/gitconnected/9-things-i-never-knew-about-itertools-python-until-recently-b98fbed92d2f"
---

이미지 태그를 아래와 같이 수정해주세요.

![image](/TIL/assets/img/2024-07-15-9ThingsINeverKnewAboutItertoolsPythonUntilRecently_0.png)

비디오 콘텐츠 실험 64일차

## 1) Batched

itertools.batched 함수(파이썬 3.12 이상에서만 사용 가능)를 사용하면 이터러블을 일괄 처리하는 것이 쉬워집니다.

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
import itertools

for i in itertools.batched('abcdefg', 3):
    print(i)

# ('a', 'b', 'c')
# ('d', 'e', 'f')
# ('g',)
```

^ 문자열을 3개씩 묶어 반복하는 코드입니다. (마지막 묶음은 원소가 부족하므로 1개의 원소만 포함됩니다)

## 2) Pairwise

itertools.pairwise 함수를 사용하면 iterable의 각 이웃하는 두 원소를 비교할 수 있습니다.

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
import itertools

for i in itertools.pairwise('abcdefg'):
    print(i)

'''
('a', 'b')
('b', 'c')
('c', 'd')
('d', 'e')
('e', 'f')
('f', 'g')
'''
```

^우리가 iterable에 있는 각 쌍 (bigram)의 요소를 순회하는 것입니다. 각 요소를 그 다음 요소와 비교해야 할 때 유용합니다.

만약 필요하다면 이 함수를 구현하는 번거로움을 피할 수 있습니다.

# 3) Accumulate

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

itertools.accumulate 함수를 사용하면 다음을 수행할 수 있습니다:

```js
import itertools

for i in itertools.accumulate('abcdefg'):
    print(i)

'''
a
ab
abc
abcd
abcde
abcdef
abcdefg
'''
```

# 4) 조합 및 순열

itertools.combinations(iterable, n) 함수를 사용하면 iterable의 요소들 중 길이 n인 모든 조합을 생성할 수 있습니다.

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

`itertools.permutations(iterable, n)` 함수를 사용하면 길이 n의 iterable 요소들의 모든 순열을 생성할 수 있어요.

참고 - 순열에서는 요소의 순서가 중요합니다. 조합에서는 요소의 순서가 중요하지 않아요.

```js
import itertools

for i in itertools.permutations([1, 2, 3, 4], 2):
    print(i, end=' ')

# (1, 2) (1, 3) (1, 4) (2, 1) (2, 3) (2, 4) (3, 1) (3, 2) (3, 4) (4, 1) (4, 2) (4, 3)
```

^ [1, 2, 3, 4]의 요소 중에서 길이가 2인 모든 순열을 찾으려면 — 여기에서 [1, 4]와 [4, 1]은 서로 다르게 간주되는 것을 주목해주세요.

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
import itertools

for i in itertools.combinations([1, 2, 3, 4], 2):
    print(i, end=' ')

# (1, 2) (1, 3) (1, 4) (2, 3) (2, 4) (3, 4)
```

^ [1, 2, 3, 4] 요소의 길이가 2인 모든 조합을 찾습니다. [4, 1]은 [1, 4]와 동일하게 간주되어 존재하지 않음을 주목해 주세요.

## 5) 곱셈

```js
for i in [1, 2]:
    for j in [3, 4]:
        for k in [5, 6]:
            print(i, j, k)

'''
1 3 5
1 3 6
1 4 5
1 4 6
2 3 5
2 3 6
2 4 5
2 4 6
'''
```

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

Markdown 형식으로 표 태그를 변경하려면 다음과 같이 하면 됩니다.

^ a triple nested for loop.

```js
from itertools import product

for i, j, k in product([1, 2], [3, 4], [5, 6]):
  print(i, j, k)

'''
1 3 5
1 3 6
1 4 5
1 4 6
2 3 5
2 3 6
2 4 5
2 4 6
'''
```

^ itertools.product를 사용한 위의 삼중 중첩 for 루프와 똑같이 작업을 수행하여 우리는 여러 중첩된 for 루프를 간단하고 우아한 한 줄의 코드로 단축할 수 있습니다.

# 6) Groupby

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

itertools.groupby 함수를 사용하면 함수를 통해 동시에 요소를 반복하고 그룹화할 수 있습니다.

```python
import itertools

words = ['apple', 'ant', 'arm', 'boy', 'bee', 'cat', 'donkey']

def condition(x):
  return x[0]  # 요소의 첫 글자를 반환

for key, group in itertools.groupby(words, condition):
    print(key, list(group))

'''
a ['apple', 'ant', 'arm']
b ['boy', 'bee']
c ['cat']
d ['donkey']
'''
```

^ 첫 글자를 기준으로 요소를 그룹화합니다 — 이를 수행하기 위해 사용자 정의 함수 condition을 전달합니다.

```python
import itertools

words = ['apple', 'ant', 'arm', 'boy', 'bee', 'cat', 'donkey']
for key, group in itertools.groupby(words, lambda x:x[0]):
    print(key, list(group))

'''
a ['apple', 'ant', 'arm']
b ['boy', 'bee']
c ['cat']
d ['donkey']
'''
```

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

^ 첫 글자를 기준으로 요소를 그룹화하는 것 (람다 x : x[0]) — 우리는 함수를 정의하기 위해 def 키워드를 사용할 필요가 없도록 작은 익명 함수인 람다 함수를 작성할 수 있습니다.

```js
import itertools

words = ['apple', 'ant', 'arm', 'boy', 'bee', 'cat', 'donkey', 'aa', 'ab']
for key, group in itertools.groupby(words, lambda x: x[0]):
    print(key, list(group))

'''
a ['apple', 'ant', 'arm']
b ['boy', 'bee']
c ['cat']
d ['donkey']
a ['aa', 'ab']
'''
```

^ 그룹은 서로 옆에 있어야 합니다 — 여기서 [`aa`, `ab`]는 앞에서 a로 시작하는 그룹에 합류하지 않습니다.

# 7) 압축

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

itertools.compress 함수는 2개의 이터러블을 사용합니다 — 주 이터러블과 True/False 또는 1/0 값을 포함하는 다른 이터러블인데 이 값들은 주 이터러블에서 생성할 요소를 결정합니다.

```js
import itertools

for i in itertools.compress('ABCD', [1,0,0,0]):
    print(i, end=' ')

# A
```

```js
import itertools

for i in itertools.compress('ABCD', [1,1,0,0]):
    print(i, end=' ')

# A B
```

```js
import itertools

for i in itertools.compress('ABCD', [1,1,1,0]):
    print(i, end=' ')

# A B C
```

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
import itertools

for i in itertools.compress('ABCD', [1,1,1,1]):
    print(i, end=' ')

# A B C D
```

```js
import itertools

for i in itertools.compress('ABCD', [1,0,1,0]):
    print(i, end=' ')

# A C
```

# 8) Zip_longest

```js
for i,j,k in zip([1,2,3,4], [5,6,7,8], [9,10]):
  print(i, j, k)

# 1 5 9
# 2 6 10
```

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

^ 일반적인 zip() 함수를 사용할 때, 한 번에 여러 이터러블을 반복할 수 있지만, 가장 짧은 이터러블에 제한을 받습니다. 예를 들어, [9, 10]가 가장 짧은 이터러블이기 때문에 zip()은 2개의 행만 생성합니다.

```js
from itertools import zip_longest

for i, j, k in zip_longest([1, 2, 3, 4], [5, 6, 7, 8], [9, 10]):
  print(i, j, k)

# 1 5 9
# 2 6 10
# 3 7 None
# 4 8 None
```

^ 이 한계를 우회하기 위해 zip_longest로 이를 대체할 수 있으며, 이 경우 가장 긴 이터러블을 통해 zip할 수 있습니다. 그러나 특정한 이터러블에서 값이 부족해지면 None이 사용됩니다.

# 9) 반복 & 사이클

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

```js
import itertools

for i in itertools.repeat('ABC'):
  print(i)

# ABC
# ABC
# ABC
# ABC

# 무한한 수의 ABC

```

^ itertools.repeat()을 사용하면 무한히 반복할 수 있습니다.

```js
import itertools

for i in itertools.cycle('ABC'):
  print(i)

# A
# B
# C
# A
# B
# C
# A
# B
# C

# 무한한 A, B, C의 순환, 그리고 다시 A로, B로, C로...

```

# 결론

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

이해하기 쉽고 명확했길 바랍니다.

# 마무리의 한 마디

만약 이 이야기가 도움이 되었고 조금이라도 지지를 표현하고 싶다면:

- 이 이야기에 대해 50번 박수를 칩니다
- 어떻게 생각하는지 댓글을 남깁니다
- 당신에게 공감되는 부분을 강조합니다

<!-- TIL 수평 -->

<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-4877378276818686"
     data-ad-slot="1549334788"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>

<script>
(adsbygoogle = window.adsbygoogle || []).push({});
</script>

이러한 조치들은 정말로 저를 도와주고, 정말 감사합니다!

제가 쓴 Ebooks: [여기를 클릭하세요!](https://zlliu.co/ebooks)

LinkedIn: [프로필 링크](https://www.linkedin.com/in/zlliu/)
