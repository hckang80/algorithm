# algorithm
알고리즘 실력 향상을 위해!!!

https://app.codility.com

### MinAvgTwoSlice

<details>
  <summary>Task description</summary>

A non-empty array A consisting of N integers is given. A pair of integers (P, Q), such that 0 ≤ P < Q < N, is called a slice of array A (notice that the slice contains at least two elements). The average of a slice (P, Q) is the sum of A[P] + A[P + 1] + ... + A[Q] divided by the length of the slice. To be precise, the average equals (A[P] + A[P + 1] + ... + A[Q]) / (Q − P + 1).

For example, array A such that:

    A[0] = 4
    A[1] = 2
    A[2] = 2
    A[3] = 5
    A[4] = 1
    A[5] = 5
    A[6] = 8
contains the following example slices:

slice (1, 2), whose average is (2 + 2) / 2 = 2;
slice (3, 4), whose average is (5 + 1) / 2 = 3;
slice (1, 4), whose average is (2 + 2 + 5 + 1) / 4 = 2.5.
The goal is to find the starting position of a slice whose average is minimal.

Write a function:

function solution(A);

that, given a non-empty array A consisting of N integers, returns the starting position of the slice with the minimal average. If there is more than one slice with a minimal average, you should return the smallest starting position of such a slice.

For example, given array A such that:

    A[0] = 4
    A[1] = 2
    A[2] = 2
    A[3] = 5
    A[4] = 1
    A[5] = 5
    A[6] = 8
the function should return 1, as explained above.

Write an efficient algorithm for the following assumptions:

N is an integer within the range [2..100,000];
each element of array A is an integer within the range [−10,000..10,000].
</details>

```js
// 1차 70% (Correctness: 60%, Performance 80%)
function solution(A) {
    const results = {
        index: null,
        average: null
    }
    A.forEach((num, index) => {
        const average = num + A[index + 1] / 2
        if (typeof results.index !== 'number') {
            results.index = index
            results.average = average
            return
        }
        
        if (results.average > average) {
            results.index = index
            results.average = average
        }
    })
    return results.index
}
```

<br>

### GenomicRangeQuery

<details>
  <summary>Task description</summary>

A DNA sequence can be represented as a string consisting of the letters A, C, G and T, which correspond to the types of successive nucleotides in the sequence. Each nucleotide has an impact factor, which is an integer. Nucleotides of types A, C, G and T have impact factors of 1, 2, 3 and 4, respectively. You are going to answer several queries of the form: What is the minimal impact factor of nucleotides contained in a particular part of the given DNA sequence?

The DNA sequence is given as a non-empty string S = S[0]S[1]...S[N-1] consisting of N characters. There are M queries, which are given in non-empty arrays P and Q, each consisting of M integers. The K-th query (0 ≤ K < M) requires you to find the minimal impact factor of nucleotides contained in the DNA sequence between positions P[K] and Q[K] (inclusive).

For example, consider string S = CAGCCTA and arrays P, Q such that:

    P[0] = 2    Q[0] = 4
    P[1] = 5    Q[1] = 5
    P[2] = 0    Q[2] = 6
The answers to these M = 3 queries are as follows:

The part of the DNA between positions 2 and 4 contains nucleotides G and C (twice), whose impact factors are 3 and 2 respectively, so the answer is 2.
The part between positions 5 and 5 contains a single nucleotide T, whose impact factor is 4, so the answer is 4.
The part between positions 0 and 6 (the whole string) contains all nucleotides, in particular nucleotide A whose impact factor is 1, so the answer is 1.
Write a function:

function solution(S, P, Q);

that, given a non-empty string S consisting of N characters and two non-empty arrays P and Q consisting of M integers, returns an array consisting of M integers specifying the consecutive answers to all queries.

Result array should be returned as an array of integers.

For example, given the string S = CAGCCTA and arrays P, Q such that:

    P[0] = 2    Q[0] = 4
    P[1] = 5    Q[1] = 5
    P[2] = 0    Q[2] = 6
the function should return the values [2, 4, 1], as explained above.

Write an efficient algorithm for the following assumptions:

N is an integer within the range [1..100,000];
M is an integer within the range [1..50,000];
each element of arrays P and Q is an integer within the range [0..N − 1];
P[K] ≤ Q[K], where 0 ≤ K < M;
string S consists only of upper-case English letters A, C, G, T.
</details>

```js
// 1차 62% O(N * M)
function solution(S, P, Q) {
    const letters = {
        A: 1,
        C: 2,
        G: 3,
        T: 4
    }
    const results = P.map((start, index) => {
        const [letter] = [...new Set(S.slice(start, Q[index] + 1))].sort()
        return letters[letter]
    })
    return results
}

// 2차 100% O(N + M)
function solution(S, P, Q) {
    const results = []
    P.forEach((start, index) => {
        const range = S.slice(start, Q[index] + 1)
        if (range.includes('A')) return results.push(1)
        if (range.includes('C')) return results.push(2)
        if (range.includes('G')) return results.push(3)
        if (range.includes('T')) return results.push(4)
    })
    return results
}
```

<br>

### CountDiv

<details>
  <summary>Task description</summary>

Write a function:

function solution(A, B, K);

that, given three integers A, B and K, returns the number of integers within the range [A..B] that are divisible by K, i.e.:

{ i : A ≤ i ≤ B, i mod K = 0 }

For example, for A = 6, B = 11 and K = 2, your function should return 3, because there are three numbers divisible by 2 within the range [6..11], namely 6, 8 and 10.

Write an efficient algorithm for the following assumptions:

A and B are integers within the range [0..2,000,000,000];
K is an integer within the range [1..2,000,000,000];
A ≤ B.
</details>

```js
function solution(A, B, K) {
    return Math.floor(B / K) - Math.ceil(A / K) + 1
}
```

<br>

### PassingCars

<details>
  <summary>Task description</summary>

A non-empty array A consisting of N integers is given. The consecutive elements of array A represent consecutive cars on a road.

Array A contains only 0s and/or 1s:

0 represents a car traveling east,
1 represents a car traveling west.
The goal is to count passing cars. We say that a pair of cars (P, Q), where 0 ≤ P < Q < N, is passing when P is traveling to the east and Q is traveling to the west.

For example, consider array A such that:

  A[0] = 0
  A[1] = 1
  A[2] = 0
  A[3] = 1
  A[4] = 1
We have five pairs of passing cars: (0, 1), (0, 3), (0, 4), (2, 3), (2, 4).

Write a function:

function solution(A);

that, given a non-empty array A of N integers, returns the number of pairs of passing cars.

The function should return −1 if the number of pairs of passing cars exceeds 1,000,000,000.

For example, given:

  A[0] = 0
  A[1] = 1
  A[2] = 0
  A[3] = 1
  A[4] = 1
the function should return 5, as explained above.

Write an efficient algorithm for the following assumptions:

N is an integer within the range [1..100,000];
each element of array A is an integer that can have one of the following values: 0, 1.
</details>

문제 해석이 잘 되지 않음...

<br>

### MissingInteger

<details>
  <summary>Task description</summary>

This is a demo task.

Write a function:

function solution(A);

that, given an array A of N integers, returns the smallest positive integer (greater than 0) that does not occur in A.

For example, given A = [1, 3, 6, 4, 1, 2], the function should return 5.

Given A = [1, 2, 3], the function should return 4.

Given A = [−1, −3], the function should return 1.

Write an efficient algorithm for the following assumptions:

N is an integer within the range [1..100,000];
each element of array A is an integer within the range [−1,000,000..1,000,000].
</details>

```js
function solution(A) {
    const sortedNumbers = [...new Set(A)]
        .filter(number => number > 0)
        .sort((a, b) => a - b)
    const [firstNumber] = sortedNumbers
    const lastNumber = [...sortedNumbers].pop()

    if (firstNumber !== 1) return 1
    if (lastNumber === sortedNumbers.length) return lastNumber + 1

    for (let i = 0; i < sortedNumbers.length; i++) {
        if (sortedNumbers[i] !== i + 1) return i + 1
    }
}
```

<br>

### MaxCounters

<details>
  <summary>Task description</summary>

You are given N counters, initially set to 0, and you have two possible operations on them:

increase(X) − counter X is increased by 1,
max counter − all counters are set to the maximum value of any counter.
A non-empty array A of M integers is given. This array represents consecutive operations:

if A[K] = X, such that 1 ≤ X ≤ N, then operation K is increase(X),
if A[K] = N + 1 then operation K is max counter.
For example, given integer N = 5 and array A such that:

    A[0] = 3
    A[1] = 4
    A[2] = 4
    A[3] = 6
    A[4] = 1
    A[5] = 4
    A[6] = 4
the values of the counters after each consecutive operation will be:

    (0, 0, 1, 0, 0)
    (0, 0, 1, 1, 0)
    (0, 0, 1, 2, 0)
    (2, 2, 2, 2, 2)
    (3, 2, 2, 2, 2)
    (3, 2, 2, 3, 2)
    (3, 2, 2, 4, 2)
The goal is to calculate the value of every counter after all operations.

Write a function:

function solution(N, A);

that, given an integer N and a non-empty array A consisting of M integers, returns a sequence of integers representing the values of the counters.

Result array should be returned as an array of integers.

For example, given:

    A[0] = 3
    A[1] = 4
    A[2] = 4
    A[3] = 6
    A[4] = 1
    A[5] = 4
    A[6] = 4
the function should return [3, 2, 2, 4, 2], as explained above.

Write an efficient algorithm for the following assumptions:

N and M are integers within the range [1..100,000];
each element of array A is an integer within the range [1..N + 1].
</details>

```js
function solution(N, A) {
    const counters = [...new Array(N)].fill(0)
    let maxCounter = 0
    let runnedMaxCounter = 0
    A.forEach(x => {
        const idx = x - 1
        if (x <= N) {
            counters[idx] = Math.max(counters[idx], runnedMaxCounter) + 1
            maxCounter = Math.max(counters[idx], maxCounter)
        } else {
            runnedMaxCounter = maxCounter
        }
    })
    const result = counters.map(counter => {
        return Math.max(counter, runnedMaxCounter)
    })
    return result
}
```

<br>

### PermCheck

<details>
  <summary>Task description</summary>

A non-empty array A consisting of N integers is given.

A permutation is a sequence containing each element from 1 to N once, and only once.

For example, array A such that:

    A[0] = 4
    A[1] = 1
    A[2] = 3
    A[3] = 2
is a permutation, but array A such that:

    A[0] = 4
    A[1] = 1
    A[2] = 3
is not a permutation, because value 2 is missing.

The goal is to check whether array A is a permutation.

Write a function:

function solution(A);

that, given an array A, returns 1 if array A is a permutation and 0 if it is not.

For example, given array A such that:

    A[0] = 4
    A[1] = 1
    A[2] = 3
    A[3] = 2
the function should return 1.

Given array A such that:

    A[0] = 4
    A[1] = 1
    A[2] = 3
the function should return 0.

Write an efficient algorithm for the following assumptions:

N is an integer within the range [1..100,000];
each element of array A is an integer within the range [1..1,000,000,000].
</details>

```js
function solution(A) {
    const set = new Set(A)
    if (set.size !== A.length) return 0
    return Math.max(...A) === A.length ? 1 : 0
}
```

<br>

### FrogRiverOne

<details>
  <summary>Task description</summary>

A small frog wants to get to the other side of a river. The frog is initially located on one bank of the river (position 0) and wants to get to the opposite bank (position X+1). Leaves fall from a tree onto the surface of the river.

You are given an array A consisting of N integers representing the falling leaves. A[K] represents the position where one leaf falls at time K, measured in seconds.

The goal is to find the earliest time when the frog can jump to the other side of the river. The frog can cross only when leaves appear at every position across the river from 1 to X (that is, we want to find the earliest moment when all the positions from 1 to X are covered by leaves). You may assume that the speed of the current in the river is negligibly small, i.e. the leaves do not change their positions once they fall in the river.

For example, you are given integer X = 5 and array A such that:

  A[0] = 1
  A[1] = 3
  A[2] = 1
  A[3] = 4
  A[4] = 2
  A[5] = 3
  A[6] = 5
  A[7] = 4
In second 6, a leaf falls into position 5. This is the earliest time when leaves appear in every position across the river.

Write a function:

function solution(X, A);

that, given a non-empty array A consisting of N integers and integer X, returns the earliest time when the frog can jump to the other side of the river.

If the frog is never able to jump to the other side of the river, the function should return −1.

For example, given X = 5 and array A such that:

  A[0] = 1
  A[1] = 3
  A[2] = 1
  A[3] = 4
  A[4] = 2
  A[5] = 3
  A[6] = 5
  A[7] = 4
the function should return 6, as explained above.

Write an efficient algorithm for the following assumptions:

N and X are integers within the range [1..100,000];
each element of array A is an integer within the range [1..X].
</details>

```js
function solution(X, A) {
    const set = new Set()
    for (let i = 0; i < A.length; i++) {
        set.add(A[i])
        if (set.size === X) return i
    }
    return -1
}
```

<br>

### TapeEquilibrium

<details>
  <summary>Task description</summary>

  A non-empty array A consisting of N integers is given. Array A represents numbers on a tape.

  Any integer P, such that 0 < P < N, splits this tape into two non-empty parts: A[0], A[1], ..., A[P − 1] and A[P], A[P + 1], ..., A[N − 1].

  The difference between the two parts is the value of: |(A[0] + A[1] + ... + A[P − 1]) − (A[P] + A[P + 1] + ... + A[N − 1])|

  In other words, it is the absolute difference between the sum of the first part and the sum of the second part.

  For example, consider array A such that:

    A[0] = 3
    A[1] = 1
    A[2] = 2
    A[3] = 4
    A[4] = 3
  We can split this tape in four places:

  P = 1, difference = |3 − 10| = 7
  P = 2, difference = |4 − 9| = 5
  P = 3, difference = |6 − 7| = 1
  P = 4, difference = |10 − 3| = 7
  Write a function:

  function solution(A);

  that, given a non-empty array A of N integers, returns the minimal difference that can be achieved.

  For example, given:

    A[0] = 3
    A[1] = 1
    A[2] = 2
    A[3] = 4
    A[4] = 3
  the function should return 1, as explained above.

  Write an efficient algorithm for the following assumptions:

  N is an integer within the range [2..100,000];
  each element of array A is an integer within the range [−1,000..1,000].
</details>

```js
function solution(A) {
    const sumParts = {
      first: 0,
      second: 0,
      total: A.reduce((sum, number) => sum + number, 0)
    }
    let { first, second, total } = sumParts
    const results = []

    for (i = 0; i < A.length - 1; i++) {
        first += A[i]
        second = total - first
        results.push(Math.abs(first - second))
    }

    return Math.min(...results)
}
```

<br>

### PermMissingElem

<details>
  <summary>Task description</summary>

  An array A consisting of N different integers is given. The array contains integers in the range [1..(N + 1)], which means that exactly one element is missing.

  Your goal is to find that missing element.

  Write a function:

  function solution(A);

  that, given an array A, returns the value of the missing element.

  For example, given array A such that:

    A[0] = 2
    A[1] = 3
    A[2] = 1
    A[3] = 5
  the function should return 4, as it is the missing element.

  Write an efficient algorithm for the following assumptions:

  N is an integer within the range [0..100,000];
  the elements of A are all distinct;
  each element of array A is an integer within the range [1..(N + 1)].
</details>

```js
function solution(A) {
    const ONE = 1
    if (!A.length) return ONE

    const sortedNumbers = [...A].sort((a, b) => a - b)
    const [firstNumber] = sortedNumbers
    if (firstNumber !== ONE) return ONE

    const result = sortedNumbers.find((num, index, origin) => {
        return !((num + origin[index + 1]) % 2)
    })
    return result + 1
}
```

<br>

### FrogJmp

<details>
  <summary>Task description</summary>

  A small frog wants to get to the other side of the road. The frog is currently located at position X and wants to get to a position greater than or equal to Y. The small frog always jumps a fixed distance, D.

  Count the minimal number of jumps that the small frog must perform to reach its target.

  Write a function:

  function solution(X, Y, D);

  that, given three integers X, Y and D, returns the minimal number of jumps from position X to a position equal to or greater than Y.

  For example, given:

    X = 10
    Y = 85
    D = 30
  the function should return 3, because the frog will be positioned as follows:

  after the first jump, at position 10 + 30 = 40
  after the second jump, at position 10 + 30 + 30 = 70
  after the third jump, at position 10 + 30 + 30 + 30 = 100
  Write an efficient algorithm for the following assumptions:

  X, Y and D are integers within the range [1..1,000,000,000];
  X ≤ Y.
</details>

```js
function solution(X, Y, D) {
    return Math.ceil((Y - X) / D)
}
```

<br>

### OddOccurrencesInArray

<details>
  <summary>Task description</summary>

  A non-empty array A consisting of N integers is given. The array contains an odd number of elements, and each element of the array can be paired with another element that has the same value, except for one element that is left unpaired.

  For example, in array A such that:

    A[0] = 9  A[1] = 3  A[2] = 9
    A[3] = 3  A[4] = 9  A[5] = 7
    A[6] = 9
  the elements at indexes 0 and 2 have value 9,
  the elements at indexes 1 and 3 have value 3,
  the elements at indexes 4 and 6 have value 9,
  the element at index 5 has value 7 and is unpaired.
  Write a function:

  function solution(A);

  that, given an array A consisting of N integers fulfilling the above conditions, returns the value of the unpaired element.

  For example, given array A such that:

    A[0] = 9  A[1] = 3  A[2] = 9
    A[3] = 3  A[4] = 9  A[5] = 7
    A[6] = 9
  the function should return 7, as explained in the example above.

  Write an efficient algorithm for the following assumptions:

  N is an odd integer within the range [1..1,000,000];
  each element of array A is an integer within the range [1..1,000,000,000];
  all but one of the values in A occur an even number of times.
</details>

```js
function solution(A) {
    const counters = A.reduce((counter, num) => {
        counter[num] = counter[num] ? counter[num] + 1 : 1
        return counter
    }, {})

    for (const key in counters) {
        if (counters[key] % 2) return +key;
    }
}
```

<br>

### CyclicRotation

<details>
  <summary>Task description</summary>

  An array A consisting of N integers is given. Rotation of the array means that each element is shifted right by one index, and the last element of the array is moved to the first place. For example, the rotation of array A = [3, 8, 9, 7, 6] is [6, 3, 8, 9, 7] (elements are shifted right by one index and 6 is moved to the first place).

  The goal is to rotate array A K times; that is, each element of A will be shifted to the right K times.

  Write a function:

  function solution(A, K);

  that, given an array A consisting of N integers and an integer K, returns the array A rotated K times.

  For example, given

      A = [3, 8, 9, 7, 6]
      K = 3
  the function should return [9, 7, 6, 3, 8]. Three rotations were made:

      [3, 8, 9, 7, 6] -> [6, 3, 8, 9, 7]
      [6, 3, 8, 9, 7] -> [7, 6, 3, 8, 9]
      [7, 6, 3, 8, 9] -> [9, 7, 6, 3, 8]
  For another example, given

      A = [0, 0, 0]
      K = 1
  the function should return [0, 0, 0]

  Given

      A = [1, 2, 3, 4]
      K = 4
  the function should return [1, 2, 3, 4]

  Assume that:

  N and K are integers within the range [0..100];
  each element of array A is an integer within the range [−1,000..1,000].
  In your solution, focus on correctness. The performance of your solution will not be the focus of the assessment.
</details>

```js
function solution(A, K) {
    const moves = K % A.length
    if (!moves || new Set(A).size === 1) return A
    return [...A.slice(-1 * moves), ...A.slice(0, A.length - moves)]
}
```

<br>

### BinaryGap

<details>
  <summary>Task description</summary>

  A binary gap within a positive integer N is any maximal sequence of consecutive zeros that is surrounded by ones at both ends in the binary representation of N.

  For example, number 9 has binary representation 1001 and contains a binary gap of length 2. The number 529 has binary representation 1000010001 and contains two binary gaps: one of length 4 and one of length 3. The number 20 has binary representation 10100 and contains one binary gap of length 1. The number 15 has binary representation 1111 and has no binary gaps. The number 32 has binary representation 100000 and has no binary gaps.

  Write a function:

  function solution(N);

  that, given a positive integer N, returns the length of its longest binary gap. The function should return 0 if N doesn't contain a binary gap.

  For example, given N = 1041 the function should return 5, because N has binary representation 10000010001 and so its longest binary gap is of length 5. Given N = 32 the function should return 0, because N has binary representation '100000' and thus no binary gaps.

  Write an efficient algorithm for the following assumptions:

  N is an integer within the range [1..2,147,483,647].
</details>

```js
function solution(N) {
    const binary = N.toString(2)
    const validGaps = binary
      .split('1')
      .slice(1, -1)
    if (!validGaps.length) return validGaps.length
    const gaps = validGaps.map(gap => gap.length)
    return Math.max(...gaps)
}
```
