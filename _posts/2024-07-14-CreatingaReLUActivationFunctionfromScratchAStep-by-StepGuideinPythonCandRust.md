---
title: "Python, C, Rust로 직접 만들어보는 ReLU 활성화 함수 단계별 가이드"
description: ""
coverImage: "/TIL/assets/img/2024-07-14-CreatingaReLUActivationFunctionfromScratchAStep-by-StepGuideinPythonCandRust_0.png"
date: 2024-07-14 23:51
ogImage: 
  url: /TIL/assets/img/2024-07-14-CreatingaReLUActivationFunctionfromScratchAStep-by-StepGuideinPythonCandRust_0.png
tag: Tech
originalTitle: "Creating a ReLU Activation Function from Scratch A Step-by-Step Guide in Python, C, and Rust"
link: "https://medium.com/@rabmcmenemy/creating-a-relu-activation-function-from-scratch-a-step-by-step-guide-in-python-c-and-rust-20e8ff6c398e"
---



![image](/TIL/assets/img/2024-07-14-CreatingaReLUActivationFunctionfromScratchAStep-by-StepGuideinPythonCandRust_0.png)

# 소개

신경망 세계에서 활성화 함수는 모델의 출력을 결정하는 데 중요한 역할을 합니다. 가장 인기 있는 활성화 함수 중 하나는 ReLU(렉티파이드 루 linear Unit)입니다. 간단함과 효과적임으로 유명한 ReLU는 많은 딥러닝 모델에서 표준 선택지가 되었습니다. 이 안내서에서는 Python, C 및 Rust 세 가지 다른 프로그래밍 언어로부터 ReLU 활성화 함수를 처음부터 만드는 과정을 안내합니다. 이를 통해 다양한 플랫폼에서의 구현과 이점에 대한 명확한 이해를 제공할 것입니다.

# ReLU란 무엇인가요?


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

ReLU은 Rectified Linear Unit의 약자입니다. 이는 입력 값을 반환하는 활성화 함수로 정의됩니다:

![ReLU Activation Function](/TIL/assets/img/2024-07-14-CreatingaReLUActivationFunctionfromScratchAStep-by-StepGuideinPythonCandRust_1.png)

더 간단히 말하면, 입력 값이 양수인 경우 입력 값을 반환하고, 그렇지 않으면 0을 반환합니다. ReLU 함수는 수학적으로 다음과 같이 표현됩니다:

![ReLU Formula](/TIL/assets/img/2024-07-14-CreatingaReLUActivationFunctionfromScratchAStep-by-StepGuideinPythonCandRust_2.png)

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

# ReLU를 사용하는 이유?

- 단순성: ReLU 함수는 0을 기준으로 한 간단한 임계값 처리를 포함하기 때문에 계산 효율적입니다.
- 비선형성: 선형 함수처럼 보이지만 ReLU는 비선형성을 도입하여 복잡한 패턴을 학습하는 데 필수적입니다.
- 희소 활성화: ReLU는 희소한 활성화를 생성하는 경향이 있어서 (많은 뉴런이 0을 출력) 네트워크를 더 효율적으로 만듭니다.

# Python에서 ReLU 및 신경망 레이어 구현하기

파이썬에서 ReLU 활성화 함수와 간단한 신경망 레이어를 구현해보겠습니다.

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

# 파이썬 구현

## 단계 1: ReLU 함수 정의

먼저, 간단한 파이썬 함수를 사용하여 ReLU 함수를 정의해 보겠습니다.

```python
def relu(x):
    return max(0, x)
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

## 단계 2: 배열에 ReLU 적용하기

우리는 NumPy를 사용하여 배열을 처리할 수 있는 ReLU 함수를 확장할 것입니다.

```python
import numpy as np

def relu_array(arr):
    return np.maximum(0, arr)
```

## 단계 3: 간단한 신경 계층 정의하기

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

자, 이제 간단한 신경망 레이어 클래스를 만들어봅시다.

```js
class SimpleNeuralLayer:
    def __init__(self, input_size, output_size):
        self.weights = np.random.randn(input_size, output_size)
        self.biases = np.zeros(output_size)

    def forward(self, inputs):
        z = np.dot(inputs, self.weights) + self.biases
        return relu_array(z)
```

## 단계 4: 신경망 레이어 테스트

```js
# 예제 사용법
layer = SimpleNeuralLayer(3, 2)
inputs = np.array([1, 2, -1])
output = layer.forward(inputs)
print(output)
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

# C에서 ReLU 및 신경 계층 구현

이제 C에서 ReLU 함수와 간단한 신경망 계층을 구현해 보겠습니다.

# C 구현

## 단계 1: ReLU 함수 정의

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
#include <stdio.h>

double relu(double x) {
    return x > 0 ? x : 0;
}
```

## 단계 2: 배열에 ReLU 적용하기

```js
#include <stdio.h>

void relu_array(double* arr, int size) {
    for (int i = 0; i < size; i++) {
        arr[i] = arr[i] > 0 ? arr[i] : 0;
    }
}
```

## 단계 3: 간단한 신경망 레이어 정의하기


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


## 단계 4: 신경망 레이어 테스트

```js
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    srand(time(NULL));
    SimpleNeuralLayer layer = create_layer(3, 2);
    double inputs[] = {1, 2, -1};
    double output[2];
    forward(layer, inputs, output);
    printf("Output: %f %f\n", output[0], output[1]);
    free(layer.weights);
    free(layer.biases);
    return 0;
}
```

# ReLU 및 신경망 레이어 구현하기


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

마침내, ReLU 함수와 간단한 신경망 레이어를 Rust로 구현해 봅시다.

# Rust 구현

## 단계 1: ReLU 함수 정의

```rust
fn relu(x: f64) -> f64 {
    if x > 0.0 { x } else { 0.0 }
}
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

## 단계 2: 배열에 ReLU 적용하기

```js
fn relu_array(arr: &mut [f64]) {
    for x in arr.iter_mut() {
        *x = if *x > 0.0 { *x } else { 0.0 };
    }
}
```

## 단계 3: 간단한 신경망 계층 정의

```js
use rand::Rng;

struct SimpleNeuralLayer {
    weights: Vec<Vec<f64>>,
    biases: Vec<f64>,
}

impl SimpleNeuralLayer {
    fn new(input_size: usize, output_size: usize) -> Self {
        let mut rng = rand::thread_rng();
        let weights = (0..input_size)
            .map(|_| (0..output_size).map(|_| rng.gen_range(-1.0..1.0)).collect())
            .collect();
        let biases = vec![0.0; output_size];
        
        SimpleNeuralLayer { weights, biases }
    }

    fn forward(&self, inputs: &[f64]) -> Vec<f64> {
        let mut output = vec![0.0; self.biases.len()];
        
        for (i, &bias) in self.biases.iter().enumerate() {
            output[i] = inputs.iter()
                .zip(&self.weights)
                .map(|(&input, weight_row)| input * weight_row[i])
                .sum::<f64>() + bias;
            output[i] = relu(output[i]);
        }
        output
    }
}
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

## 단계 4: 신경 계층 테스트

```js
fn main() {
    let layer = SimpleNeuralLayer::new(3, 2);
    let inputs = [1.0, 2.0, -1.0];
    let output = layer.forward(&inputs);
    
    for value in output {
        print!("{} ", value);
    }
    // 출력: 0 0
}
```

# 결론

ReLU 활성화 함수를 만들고 간단한 신경망 계층에 통합하는 것은 신경망 작업에 대한 기본 개념을 강조합니다. Python, C 및 Rust에서 ReLU를 구현함으로써, 여러 플랫폼에서 딥러닝 모델의 성공을 이끌어내는 주요 구성 요소 중 하나에 대한 통찰을 얻을 수 있습니다.

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

기계 학습 초보자이거나 경험이 풍부한 실무자이든 ReLU와 같은 활성화 함수의 내부 작업을 이해하는 것이 중요합니다. 이 지식을 통해 다양한 응용 프로그램을 위해 신경망을 더 잘 설계, 디버그 및 최적화할 수 있습니다.

좋은 코딩되세요!

![image](/TIL/assets/img/2024-07-14-CreatingaReLUActivationFunctionfromScratchAStep-by-StepGuideinPythonCandRust_3.png)