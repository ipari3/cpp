## complement
보수는 두 양수 이진수의 뺄셈에서 음수 역할을 하는 수다.
- 뺄셈 (a - b)을 덧셈 (a + c)으로 만든다.
- c가 보수이며 (-b)의 역할을 한다.  
다만 -부호를 가진 값이 아니라, +부호를 가지고 음수의 역할을 하는 것이다.
다른 덧셈 및 뺄셈 경우의 수는 보수를 이용할 필요가 없다.
- 두 양수의 덧셈은 그냥 더하면 된다.
- 두 음수의 덧셈은 그냥 더하고 -부호를 붙이면 된다.
#### ones' complement
1들의 보수는 각 비트를 반전시켜서 만든다. (보통 1의 보수라고 한다.)  
부호 전환은 간단하지만 뺄셈이 복잡하다.
##### 부호 전환
- 0000 0111 (7)
- 1111 1000 (-7)
##### 뺄셈
case 1. 자리올림이 발생한 경우: 뺄셈 결과가 양수다.  
예시) 8 - 7 = 1
```
step 1. 빼는 수의 보수인 -7을 구한다.
위에서 구했듯이 -7은 1111 1000이다.

step 2. 8 + (-7)로 계산한다.
  0000 1000 (8)
+ 1111 1000 (-7)
-----------------
 10000 0000

step 3. 위 결과의 최하위 비트에 1을 더한 것이 뺄셈 결과다.
  0000 0001 (1)
```

case 2. 자리올림이 발생하지 않는 경우: 뺄셈 결과가 0 또는 음수인 경우  
예시) 7 - 7 = 0
```
step 1. 빼는 수의 보수인 -7을 구한다.
위에서 구했듯이 -7은 1111 1000이다.

step 2. 7 + (-7)로 계산한다.
  0000 0111 (7)
+ 1111 1000 (-7)
-----------------
  1111 1111

step 3. 위 결과의 1들의 보수를 구하고 - 부호를 붙인 것이 뺄셈 결과다.
 -0000 0000 (-0)
```
- 1들의 보수와의 합은 모든 비트가 1인 값이다.
## zero-extended