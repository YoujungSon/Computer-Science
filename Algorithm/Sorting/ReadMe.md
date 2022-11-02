# What is sorting?

- sorting이란, collection(e.g. array)의 item들을 재정렬하는 과정을 뜻한다. 즉, 이 과정의 결과 item들은 특정한 순서를 갖게된다.
- 오름차순, 내림차순, 알파벳순 등..

```
[23, 45, 67, 6, 13]

=> [6, 13, 23, 45, 67]

```

정렬에는 매우 다양한 방법이 있고, 상황에 따라 적절한 정렬 알고리즘을 선택하여 사용할 수 있어야 한다.

<BR >

# Bubble Sort (버블 정렬)

- 시간 복잡도 : 평균 O(n^2)
- 루프를 돌면서 각 항목을 다음 항목(해당 항목의 오른쪽에 있는 항목)과 비교하고 교환하는 것.
- 오른쪽부터 정렬시킴.
- 반복할 때마다 정렬해야 할 항목이 줄어든다.
- 효율적이지 않고, 잘 사용되지 않음.

```js
function bubbleSort(arr) {
  for (let i = arr.length; i > 0; i--) {
    //배열의 끝에서부터 정렬되기 때문에 길이에서 하나씩 줄여가며 반복문
    for (let j = 0; j < i - 1; j++) {
      //j번째와 j+1번째를 비교
      if (arr[j] > arr[j + 1]) {
        //swap
        let temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
  }
  return arr;
}

bubbleSort([23, 45, 67, 6, 13]); //[6, 13, 23, 45, 67]
```

중첩반복문을 쓰는 건, n 바퀴를 돌면서, 그 안에서 계속 숫자를 비교해나가야하기 때문.
배열의 끝에서부터 정렬이 되기때문에, i를 배열의 길이에서부터 하나씩 줄여가며 반복문을 돌린다.
j는 j 번째와 j+1 번째를 비교하기 위해 반복문을 돌린다.

최적화 ver.

```js
function bubbleSort(arr) {
  let noSwap;
  for (let i = arr.length; i > 0; i--) {
    noSwap = true;
    for (let j = 0; j < i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        //swap
        let temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
        noSwap = false; //swap이 일어난다면 false로 값 변경
      }
    }
    if (noSwap) break; //swap이 일어나지 않았다 === 정렬이 완료 되었다 => 종료
  }
  return arr;
}

bubbleSort([8, 1, 2, 3, 4]);
```

# Selection sort(선택 정렬)

- 시간 복잡도 : 평균 O(n^2)
- 버블정렬과 비슷하지만, 차이점은 앞에서부터 정렬된다는 것.
- 원소를 넣을 위치는 이미 정해져있고, 어떤 원소를 넣을지 선택하는 알고리즘
- 오름차순 정렬이라면, 최솟값을 찾아서 정렬하는 알고리즘
- 비효율적

```js
function selectionSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    //배열의 길이만큼 반복문을 돌면서
    let lowest = i; //최솟값의 인덱스를 찾아낼 것이고, 초기값은 제일 첫번째 값인 i
    for (let j = i + 1; j < arr.length; j++) {
      // j는 i+1의 인덱스부터
      if (arr[j] < arr[lowest]) {
        //j번째 값이 최솟값보다 작다면 j가 최솟값의 인덱스가 됨
        lowest = j;
      }
    }
    // 반복문이 끝났을 때
    if (i !== lowest) {
      //최솟값의 인덱스가 초기값인 i라면 swap하지 않음
      // swap!
      let temp = arr[i];
      arr[i] = arr[lowest];
      arr[lowest] = temp;
    }
  }
  return arr;
}

selectionSort([23, 45, 67, 6, 13]); //[6, 13, 23, 45, 67]
```

# Insertion sort(삽입 정렬)

- 시간 복잡도 평균 O(n^2)
- 가장 앞을 정렬된 것으로 보고, 그 사이에 삽입해가며 정렬하는 것.
- 2번째 원소부터 시작하여서 그 앞(왼쪽)의 원소들과 비교하여 삽입할 위치를 정한 뒤, 지정된 자리에 자료를 삽입
- 대부분의 원소가 이미 정렬되어 있는 경우 매우 효율적이다.

```js
function insertionSort(arr) {
  let currentValue;
  //정렬할 부분
  for (i = 1; i < arr.length; i++) {
    currentVal = arr[i];
    for (j = i - 1; j >= 0 && arr[j] > currentVal; j--) {
      //j는 정렬된 어레이 중 가장 오른쪽 값(큰값), currentValue가 정렬된 어레이의 값보다 작아야 루프 안으로 들어올 수 있음
      arr[j + 1] = arr[j];
    }
    arr[j + 1] = currentVal;
  }
  return arr;
}
insertionSort([2, 5, 7, 3]); // [2, 3, 5, 7]
```

- i는 정렬이 안된 부분을 가장 왼쪽부터 돌 때 사용하는 변수
- j는 정렬된 부분을 가장 오른쪽부터 돌 때 사용하는 변수

```
[7, 2, 5, 3];

// 첫번째 - 1
// i = 1, j =0 , currentValue = 2
j >= 0 && arr[j] > curentValue; //O
arr[j + 1] = arr[j]; //[7, 7, 5, 3]

// 첫번째 - 2
// i =1, j = -1, currentValue = 2
j >= 0 && arr[j] > curentValue; //X
arr[j + 1] = curentValue; // arr[0] = 2   // [2, 7, 5, 3]

// 두번째 -1
// i = 2, j = 1, currentValue = 5
j >= 0 && arr[j] > curentValue; // O
arr[j + 1] = arr[j]; // [2, 7, 7, 3]

// 두번째 -2
// i=2, j=0, currentValue = 5
j >= 0 && arr[j] > curentValue; //X
arr[j + 1] = curentValue; // arr[1] = 5  // [2, 5, 7, 3]
```

# Merge sort (합병 정렬)

- 시간 복잡도 평균 O(n log n) 최악 O(n log n)
- 분할 정복 알고리즘
- 빠르고 효율적이기 때문에, 대규모의 데이터를 다룰 때 좋음.
- 배열을 더 작은 배열로 나누는 방식
- 배일에 0개 혹은 1 개의 요소가 있다면 그것은 정렬된 배열이라는 점을 이용
- 정렬된 배열을 합치는 것은 정렬되지 않은 배열을 합치는 것보다 쉽다는 점을 이용

입력 받은 두 배열에서 작은 값을 먼저 새로운 result라는 배열에 push 하는 함수

```js
function merge(arr1, arr2) {
  let results = [];
  let i = 0;
  let j = 0;
  while (i < arr1.length && j < arr2.length) {
    if (arr2[j] > arr1[i]) {
      results.push(arr1[i]);
      i++;
    } else {
      results.push(arr2[j]);
      j++;
    }
  }
  while (i < arr1.length) {
    results.push(arr1[i]);
    i++;
  }
  while (j < arr2.length) {
    results.push(arr2[j]);
    j++;
  }
  return results;
}
merge([1, 15, 50] , [2, 10]) // [1, 2, 10, 15, 50]
```



```js
function mergeSort(arr) {
  if (arr.length <= 1) return arr;
  let mid = Math.floor(arr.length / 2);
  let left = mergeSort(arr.slice(0, mid));
  let right = mergeSort(arr.slice(mid));
  return merge(left, right);
}

mergeSort([10, 24, 76, 73]); //[10, 24, 73, 76]
```

```
[10, 24, 76, 73]
=> left = mergeSort([10, 24]), right = mergeSort([76, 73]
=> left = mergeSort[10] , rigth = mergeSort([24])
=> left = [10] right = [24]
=> left = [10, 24], right = mergeSort([76, 73])
=> left = mergeSort([76]), right = mergeSort([73])
=> left = [76], right = [73]
=> left = [10, 24], right = [73, 76]
=> [10, 24, 73, 76]
```

<br/>

# Quick sort(퀵 정렬)

- 시간 복잡도 : 평균 O(n log n), 최악 O(n^2)
- 배열에 0개 혹은 1 개의 요소가 있다면 그것은 정렬된 배열이라는 점을 이용
- pivot 이라 불리는 하나의 요소를 선택하여 그에 맞게 왼쪽/오른쪽으로 정렬을 함.
- pivot보다 작은 모든 원소는 왼쪽으로, pivot보다 큰 모든 원소는 오른쪽으로 이동. (하지만 이동된 원소들의 사이의 순서는 보장하지 않음.)
- 이상적으로 pivot은 중간값이다. 하지만 데이터 순서를 모른다면 무작위 요소를 선택함.
- 비교를 하면서 pivot보다 작은 수가 나오면 피벗인덱스를 하나씩 늘려가고, 해당 loop가 끝났을 때 피벗인덱스 번호의 수와 피벗을 swap

```
[4,6,9,1,2,5,3]
       4
 3,2,1    6,9,5
     3      6
 2,1      5  9
   2
 1
[1,2,3,4,5,6,9]
```

첫번째 요소를 pivot으로 취하고, 왼쪽, 오른쪽으로 정렬한 뒤, pivot index를 반환하는 함수

```js
function pivot(arr, start = 0, end = arr.length + 1) {
  function swap(array, i, j) {
    let temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }

  let pivot = arr[start];
  let swapIdx = start;

  for (let i = start + 1; i < arr.length; i++) {
    if (pivot > arr[i]) {
      swapIdx++;
      swap(arr, swapIdx, i);
    }
  }
  swap(arr, start, swapIdx);
  return swapIdx;
}
pivot([4, 6, 9, 1, 2, 5, 3]); //3
```

```js
function quickSort(arr, left = 0, right = arr.length - 1) {
  if (left < right) {
    let pivotIndex = pivot(arr, left, right); //3
    //left
    quickSort(arr, left, pivotIndex - 1);
    //right
    quickSort(arr, pivotIndex + 1, right);
  }
  return arr;
}

quickSort([4, 6, 9, 1, 2, 5, 3]); //[1, 2, 3, 4, 5, 6, 9]
```
