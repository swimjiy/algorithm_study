# CH06 정렬

## 06-1 정렬

### 정렬

정렬은 핵심 항목(key)의 대소관계에 따라 데이터 집합을 일정한 순서로 줄지어 늘어서도록 바꾸는 작업. 정렬은 대표적인 알고리즘이 8개가 있으며, 안정된 알고리즘과 그렇지 않은 알고리즘으로 나뉜다.

### 내부 정렬 & 외부 정렬

- 내부 정렬: 하나의 배열에서 작업할 수 있는 경우
- 외부 정렬: 하나의 배열에서 작업할 수 없는 경우

책에서 다루는 모든 정렬은 내부 정렬이다.

### 정렬 알고리즘의 핵심 요소

교환, 선택, 삽입.

## 06-2 버블 정렬

버블 정렬은 이웃한 두 요소이 대소 관계를 비교하여 교환을 반복한다.

한쪽 끝에서부터 인접한 두 요소를 비교하면서 오름차순이면 큰 걸 뒤로, 내림차순이면 작은 걸 뒤로 보내면서 배열을 끝까지 돈다. 이 때, 이렇게 서로 비교 및 교환하는 일련의 작업을 패스(pass)라고 부른다. 한번의 패스가 끝이나면 다시 처음으로 돌아와서 같은 걸 반복하지만, 맨 마지막 요소에는 이미 가장 크거나 작은 것이 자리하고 있을 것이기 때문에 패스를 하는 횟수가 매번 1회씩 줄어든다. 버블 정렬에서 패스는 총 n-1회 수행된다.

```java
for (int i = 0; i < n - 1; i++)
	for (int j = n - 1; j > i; j--)
		if (a[j - 1] > a[j])
			swap(a, j - 1, j);
```

**알고리즘 개선 1**

패스를 수행하다가 이미 정렬된 순간에도, 위와 같은 형식이라면 루프문이 종료될 때까지 돌게 된다. 이를 방지하기 위해서, 매 패스마다 교환 횟수를 저장하고 이게 0이면 정지하면 된다.

```java
for (int i = 0; i < n - 1; i++) {
	int exchange = 0;
	for (int j = n - 1; j > i; j--)
		if (a[j - 1] > a[j]) {
			swap(a, j - 1, j);
			exchange++;
		}
	if (exchange == 0) break;
}
```

**알고리즘 개선2**

정렬을 하다보면, 일정 범위까지는 이미 정렬이 종료된 경우도 있다. 그러면 굳이 그 부분은 비교가 필요 없기 때문에, 몇번째 요소까지는 정렬이 불필요한지 체크해서 루프문을 적게 도는 것도 방법이다.

```java
int k = 0;
while (k < n - 1) {
	int last = n - 1;
	for (int j = n - 1; j > k; j--)
		if (a[j - 1] > a[j]) {
			swap(a, j - 1, j);
			last = j;
		}
	k = last;
}
```

## 06-3 단순 선택 정렬

단순 선택 정렬은 가장 작은 요소부터 선택해 알맞은 위치로 옮겨서 순서대로 정렬하는 알고리즘이다. 교환 과정은 아래와 같다.

1. 아직 정렬하지 않은 부분에서 가장 작은 키의 값을 선택한다.
2. 이 값과 아직 정렬하지 않은 부분의 첫 번째 요소를 교환한다.

이 과정을 n-1회 반복하면 된다.

```java
for (int i = 0; i < n - 1; i++) {
	int min = i;
	for (int j = i + 1; j < n; j++)
		if (a[j] < a[min])
			min = j;
	swap(a, i, min);
}
```

## 06 - 4 단순 삽입 정렬

선택한 요소를 그보다 더 앞쪽의 알맞은 위치에 삽입하는 작업을 반복하여 정렬하는 알고리즘이다. 두번째 요소부터 시작해서 앞에 정렬된 것들과 비교하면서 적절한 위치에 넣는 형태라고 보면 된다.

```java
for (int i = 1; i < n; i+) {
	int j;
	int tmp = a[i];
	for (j = i; j > 0 && a[j - 1] > tmp; j--)
		a[j] = a[j - 1];
	a[j] = tmp;
}
```

#### 시간 복잡도

이렇게 6-2부터 6-4까지 등장한 세 가지 단순 정렬(버블, 선택, 삽입)의 시간 복잡도는 모두 O(n^2). 효율이 좋은 방법은 아니라고 한다. 

## 06-5 셸 정렬

셸 정렬은 단순 삽입 정렬의 장점은 살리고 단점은 보완한 알고리즘이다. 단순 삽입 정렬의 특징은 아래와 같다. 

- 장점: 정렬을 마쳤거나 정렬을 마친 상태에 가까우면 정렬 속도가 매우 빨라진다.
- 단점: 삽입할 위치가 멀리 떨어져 있으면 이동해야 하는 횟수가 많아 비효율적이다. 

셸 정렬은 이런 장점은 살리고 단점은 보완한 방법이라고 한다. 요약한다면, 배열의 요소를 2개, 4개, 8개, ... 식으로 짝을 지어서 미리미리 정렬을 해둔 뒤에, 이렇게 최대한 정렬된 상황의 배열을 가지고 단순 삽입 정렬을 수행한다는 개념이다. 

```java
for (int h = n / 2; h > 0; h /= 2) {
  for (int i = h; i < n; i++) {
    int j;
    int tmp = a[i];
    for (j = i - h; j >= 0 && a[j] > tmp; j -= h)
      a[j + h] = a[j];
    a[j + h] = tmp;
  }
}
```

#### 중분값(h) 설정

한 가지 주목할 점은, 배열을 나누는 기준이 되는 숫자 h이다. 책에서는 4, 2, 1의 순서로 길이 8의 배열을 정리하는 예시를 소개하고 있는데, 이렇게 할 경우 절대 섞이지 않게 되는 요소들이 존재한다. 그렇게 되면 정렬을 해준다 한들, 결국 큰 변화가 없어서 효율이 떨어질 수 있다고 한다. 이를 보완하기 위해서 h의 값은 서로 배수가 되지 않는 것이 중요하다고 한다. 이런 점을 고려하여 다음과 같이 코드를 개선해볼 수 있다. 

```java
int h;
for (h = 1; h < n / 9; h = h * 3 + 1) ;

for ( ; h > 0; h /= 3) {
  for (int i = h; i < n; i++) {
    int j;
    int tmp = a[i];
    for (j = i - h; j >= 0 && a[j] > tmp; j -= h)
      a[j + h] = a[j];
    a[j + h] = tmp;
  }
}

```



## 06-6 퀵 정렬

퀵 정렬은 가장 빠른 정렬 알고리즘 중의 하나로 알려져 있다. 간단한 원리는, 배열에서 어떠한 기준값(피벗)을 정하고 이를 기준으로 더 큰 값과 작은 값으로 그룹을 나눈다. 이렇게 나뉜 그룹 안에서 또 피벗을 설정하고 큰값, 작은값으로 나누는 것을 반복한다. 이렇게 계속 배열을 나누다가 모든 그룹이 1이 되면 정렬을 마치게 된다. 

#### 배열을 두 그룹으로 나누기

퀵소트는 피벗값의 인덱스와, 배열의 양 끝에서부터 피벗을 향해 이동하는 두 개의 인덱스 값이 필요하다. left, right라고 명명하고 생각하면 다음과 같다. (오름차순으로 정렬한다고 생각하면)

피벗과 비교해가면서 왼쪽에서 피벗을 향해 오다가 피벗보다 큰 값을 만나면 해당 인덱스에 잠시 멈춰있는다. 그 다음엔 반대로 오른쪽에서 피벗을 향해 오다가 피벗보다 작은 값을 만나면 해당 인덱스에서 멈춘다. 이렇게 멈췄을 때 left가 right보다 크거나 같지 않다면 두 위치의 값을 바꿔준다. left >= right 가 아닐 때까지 마저 이동하면서 바꾸는 작업을 진행한다. 

퀵 소트는 이렇게 피벗을 기준으로 나눠진 그룹들이 각각 또 위와 같은 로직으로 각자의 피벗을 기준으로 정렬을 하다가 모든 그룹의 크기가 1이 되면 완전히 정렬을 마친다. 이런 성질 때문에, 재귀를 활용하여 퀵소트의 함수를 짤 수 있다.

```java
static void quickSort(int[] a, int left, int right) {
  int pl = left;
  int pr = right;
  int x = a[(pl + pr) / 2];
  
  do {
    while (a[pl] < x) pl++;
    while (a[pr] > x) pr--;
    if (pl <= pr)
      swap(a, pl++, pr--);
  } while (pl <= pr);
  
  if (left < pr)	quickSort(a, left, pr);
  if (pl < right)	quickSort(a, pl, right);
  }
}
```



#### 비재귀적인 퀵 정렬

```java
static void quickSort(int[] a, int left, int right) {
  IntStack lstack = new IntStack(right - left + 1);
  IntStack rstack = new IntStack(right - left + 1);
  
  lstack.push(left);
  rstack.push(right);
  
  while (lstack.isEmpty() != true) {
    int pl = left = lstack.pop();
    int pr = right = rstack.pop();
    int x = a[(left + right) / 2];
    
    do {
    while (a[pl] < x) pl++;
    while (a[pr] > x) pr--;
    if (pl <= pr)
      swap(a, pl++, pr--);
  	} while (pl <= pr);
    
    if (left < pr) {
      lstack.push(left);
      rstack.push(pr);
    }
    if (pl < right) {
      lstack.push(pl);
      rstack.push(right);
    }
  }
}
```



#### 스택의 용량

스택을 이용하여 퀵소트를 진행할 때는, 스택에 계속해서 분할된 영역을 나타내는 데이터가 쌓이게 된다. 이 때, 요소의 개수가 많은 그룹을 먼저 나누기보다, 적은 그룹부터 나누면 한번에 스택에 저장되는 양이 줄 수 있다. 그렇기에 요소의 개수가 많은 그룹을 먼저 푸시해두고, 적은 그룹을 먼저 팝해와서 분할하는 방식으로 진행하게 되면 요소의 개수가 백만개여도 스택의 최대 용량은 20개면 충분하다고 합니다. 

#### 피벗 선택하기

피벗을 잘 선택하는 것 또한 퀵정렬의 속도를 높힐 수 있는 방법이라고 한다. 

한 가지 방법으로는 배열의 요소 수가 3개 이상이라면, 임의의 3개의 요소를 선택하고 그 중에서 중앙값인 요소를 피벗으로 선택할 수 있다. 

이 방법을 조금 더 확장시킨 방법은 아래와 같다. 

나눌 배열의 처음, 가운데, 끝 요소르르 정렬한 다음 가운데 요소와 끝에서 두 번째 요소를 교환한다. 끝에서 두 번째 요소의 값(a[right - 1])을 선택하여 나눌 대상의 범위를 a[left + 1] ~ a[right - 2]로 좁힙니다. 

#### 시간 복잡도

퀵 정렬은 O(n log n)이다. 단, 피벗의 선택이나 배열의 초깃값에 따라 당연히 변동은 존재한다. 최악의 시간 복잡도는 O(n^2).



## 06-7 병합정렬

병합정렬은 배열을 앞부분과 뒷부분으로 나누어 각각 정렬한 다음 병합하는 작업을 반복하는 알고리즘이다. 배열을 앞뒤로 나누어서 각각을 정렬하고 이 둘을 나란히 보면서 더 작은 걸 최종 배열에 하나씩 집어넣는 개념이라고 생각하면 된다. 

병합 정렬 또한 분할 정복법에 따라 정렬하게 된다. 

1. 배열의 앞부분을 병합 정렬로 정렬한다.
2. 배열의 뒷부분을 병합 정렬로 정렬한다. 
3. 배열의 앞부분과 뒷부분을 병합한다. 

```java
class MergeSort {
  static int[] buff; // 작업용 배열
  
  // a[left] ~ a[right]를 재귀적으로 병합 정렬
  static void __mergeSort(int[] a, int left, int right) {
    if (left < right) {
      int i;
      int center = (left + right) / 2;
      int p = 0;
      int j = 0;
      int k = left;
      
      __mergeSort(a, left, center);
      __mergeSort(a, center + 1, right);
      
      for (i = left; i <= center; i++)
        buff[p++] = a[i];
      
      while (i <= right && j < p)
        a[k++] = (buff[j] <= a[i]) ? buff[j++] : a[i++];
      
      while (j < p)
        a[k++] = buff[j++];
    }
  }
  
  // 병합 정렬
  static void mergeSort(int[] a, int n) {
    buff = new int[n];
    
    __mergeSort(a, 0, n - 1);
    
    buff = null;
  }
}
```

#### 시간 복잡도

배열 병합의 시간 복잡도는 O(n)이고 데이터의 요소 개수가 n개일 때, 병합 정렬의 단계는 log n만큼 필요하므로 전체 시간 복잡도는 O(n log n)이라고 할 수 있다.



## 6-8 힙 정렬

힙은 '부모의 값이 자식의 값보다 항상 크다'는 조건을 만족하는 완전이진트리다. 힙 정렬은 이런 힙을 사용하여 정렬하는 알고리즘이다. 

힙 정렬은 가장 큰 값이 루트(트리에서 가장 위에 있는 노드)에 위치하는 특징을 이용한 정렬 알고리즘으로, 힙에서 가장 큰 값인 루트를 꺼내는 작업을 반복하고 그 값을 늘어놓으면 배열은 정렬을 마치게 된다. 즉, 힙 정렬은 선택 정렬을 응용한 알고리즘이며 힙에서 가장 큰 값인 루트를 꺼내고 남은 요소에서 다시 가장 큰 값을 구해야 한다. 



## 06-9 도수 정렬

도수 정렬은 요소의 대소 관계를 비교하는 대신, 누적 도수분포표를 만들어 정렬에 활용하는 방식이다. 

정수형 배열이라고 했을 때, 요소의 실제 값이 일종의 키값이 되어 빈 배열(도수분포표가 될)의 인덱스가 된다(예를 들어 숫자 1이라면 1번 인덱스). 이렇게 분포를 다 저장한 뒤, 배열을 앞에서부터 돌면서 누적해서 쌓아나간다. 그 다음엔 정렬된 형태로 담을 빈 배열을 하나 더 만들고, 원 배열을 뒤에서부터 살펴본다. 원 배열의 요소 값을 이용해 누적 도수분포표의 값을 찾고, 새로 만든 배열에서 해당 값에서 1을 뺀 인덱스의 위치에 요소 값을 저장한다.  

```java
class Fsort {
  static void fSort(int[] a, int n, int max){
    int[] f = new int[max + 1]; // 누적도수
    int[] b = new int[n];
    
    for (int i = 0; i < n; i++)				f[a[i]]++;
    for (int i = 1; i <= max; i++)		f[i] += f[i - 1];
    for (int i = n - 1; i >= 0; i--)	b[--f[a[i]]] = a[i];
    for (int i = 0; i < n; i++)				a[i] = b[i];
  }
}
```

원 배열을 역순으로 도는 이유는, 중복되는 값이 있을 경우, 뒤에 등장하는 것을 뒤에 두기 위함이라고 한다. 

도수 정렬은 데이터의 비교, 교환 작업이 필요하지 않기 때문에 매우 빠르다. 단, 도수분포표가 필요하기 대문에 최솟값, 최댓값을 미리 알고 있어야 사용이 가능하다. 





















