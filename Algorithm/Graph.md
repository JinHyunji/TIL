# 1. 그래프 기본

## 그래프
- 아이템들과 이들 사이의 연결 관계 표현
- 정점들의 집합과 이들을 연결하는 간선들의 집합으로 구성된 자료구조
- 선형 자료구조나 트리로 표현하기 어려운 M : N의 관계를 표현한 것
- V개의 정점을 가지는 그래프는 최대 V * (V - 1) / 2 간선이 가능

<br>

## 그래프 종류
- 무향 그래프 (Undirected Graph) & 유향 그래프 (Directed Graph)
- 가중치 그래프 (Weighted Graph)
- 순환 그래프 (Cycle Graph)
- 비 순환 방향 그래프 (DAG, Directed Acyclie Graph) 등

<br>

## 완전 그래프 & 부분 그래프
- 정점들에 대해 가능한 모든 간선들을 가진 그래프
- 일부 간선들을 가진 그래프
- 모든 정점이 인접

<br>

## 경로 (Path)
- 간선들을 순서대로 나열한 것
- 하나의 정점을 한 번만 지나는 경로를 단순 경로라고 한다.
- 시작 정점에서 끝나는 경로를 사이클이라고 한다.

<br>
<br>
<br>
<br>

# 2. 그래프 표현

## 그래프 표현 방법
- 간선의 정보를 저장하는 방식 : 메모리나 성능을 고려하여 결정
    1. 인접 행렬
    2. 인접 리스트
    3. 간선 배열

<br>

## 인접 행렬
- 두 정점을 연결하는 간선의 유무를 행렬로 표현
- V * V 개의 2차원 배열
- 행 번호와 열 번호는 그래프의 정점 번호
- 두 정점이 인접되어 있으면 1, 그렇지 않으면 0으로 표현 (가중치가 있다면 해당 값으로 작성)
- 무향 그래프 → i 번째 행의 합 = i 번째 열의 합 = Vi 의 진출 차수, i 번째 열의 합 = Vi 의 진입 차수

```Java
import java.util.Scanner;

public class 그래프_01_인접행렬 {
	
	static int[][] map;
	static int V, E; // 정점의 개수, 간선의 개수
	
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		V = sc.nextInt();
		E = sc.nextInt();
		
		map = new int[V][V]; // 정점의 시작점이 0인지 1인지에 따라 배열 생성 크기가 달라짐
		
		for (int i = 0; i < E; i++) {
			int A = sc.nextInt();
			int B = sc.nextInt();
			
			map[A][B] = map[B][A] = 1; // 간선의 정보를 받아서 연결된 정점 표시해주기
		}
	}
}
```


<br>

## 인접 리스트
- 각 정점에 대한 인접 정점들을 순차적으로 표현
- 하나의 정점에 대한 인접 정점들을 각 노드로 하는 연결 리스트로 저장
- 무 방향 그래프
    - 노드 수 = 간선 수 * 2
    - 각 정점의 노드 수 = 정점의 차수
- 방향 그래프
    - 노드 수 = 간선 수
    - 각 정점의 노드 수 = 정점의 진출 차수

```Java
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class 그래프_02_인접리스트 {
	
	static List<Integer>[] map; // 리스트 타입의 배열 생성
	static int V, E; // 정점의 개수, 간선의 개수
	
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		map = new ArrayList[V];
		
		for (int i = 0; i < E; i++) {
			int A = sc.nextInt();
			int B = sc.nextInt();
			
			map[A].add(B);
			map[B].add(A);
		}
	}
}
```

<br>

## 간선 배열
- 정점과 정점의 연결 정보인 간선을 배열에 저장
- 간선을 표현하는 두 정점의 정보를 배열 혹은 객체로 저장할 수 있음


<br>
<br>
<br>
<br>

# 3. 그래프 탐색

## 탐색 기법

- 모든 노드를 빠짐 없이 탐색하는 두 가지 방법 : 완전 탐색
    1. 깊이 우선 탐색 (Depth First Search, DFS)
    2. 너비 우선 탐색 (Breadth First Search, BFS)

<br>
<br>
<br>
<br>

# 4. 깊이 우선 탐색 (Depth First Search, DFS)

## DFS 란?

- 깊이 우선 탐색
- 시작 지점에서 출발하여 한 방향으로 탐색함
- 진행할 수 없다면 마지막에 만난 지점으로 돌아와 다른 방향 다시 탐색
- 후입선출(LIFO : Last-In First-Out) 구조의 스택 사용
- 재귀함수는 System Stack을 활용하므로 간단하게 구현 가능

<br>

## 트리 탐색

### while 문
1. 루트 노드 → Stack Push
2. Stack → Empty 가 될 때까지 반복 수행
    1. 현재 노드 ← Stack pop
    2. 현재 노드의 모든 자식 → Stack Push

<br>

```Java
// v : 루트 노드
	DFS(v) {
		stack.push(v);
		while (not stack.isEmpty) {
			curr = stack.pop
			for w in (curr의 모든 자식)
				stack.push(w)
		}
	}
```

<br>

### 재귀 함수
    1. 현재 노드(V) 방문
    2. V의 자식 노드(W)를 차례로 재귀 호출

<br>

```Java
// v : 루트 노드
	DFS(v) {
		v 방문;
		for w in (v의 모든 자식) {
			DFS(w);
		}
	}
```

<br>

## 그래프 탐색 (재귀 함수)

<br>

```java
// G : 그래프, visited : 방문 배열
	DFS(v) {
		visited[v] <- true // v 방문 설정
		
		for each all w in adjacency(G, v);
			if visited[w] != true
				DFS(w)
	}
```

<br>

## 미로 찾기

<br>

## 2차원 배열 탐색 방법 (재귀 함수)

<br>

```java
// arr : 2차원 배열, visited : 방문 배열
	DFS(r, c) {
		visited[r][c] <- true // v 방문 설정
		for (r, c)를 기준으로 4방향 탐색
			if 다음 좌표는 이동 가능한 것인지 체크
				DFS(nr, nc)
	}
```

<br>
<br>
<br>
<br>

# 5. 너비 우선 탐색 (Breadth First Search, BFS)

## BFS 란?

- 너비 우선 탐색
- 시작 지점에 인접한 순으로 탐색을 시작함
- 인접한 지점을 모두 방문하였다면 다음으로 인접한 지점을 방문함
- 선입선출(FIFO : First-In First-Out) 구조의 Queue 자료구조를 사용
- 너비 우선 탐색은 인접한 지점부터 방문을 하므로 시작 지점과 끝 지점이 주어졌을 때 최단 길이를 구할 수 있음

<br>

## 트리 탐색

1. 루트 노드 Queue에 삽입
2. Queue가 공백이 될 때까지 반복 수행
    1. Queue에서 원소(Curr) 꺼내기
    2. 해당 원소 방문
    3. Curr의 자식 노드 Queue에 삽입

<br>

```java
BFS(v) {
		Queue 생성;
		Queue.add(v);
		while (!Queue.isEmpty()) {
			curr <- Queue.deq();
			curr 방문
			for w (curr의 모든 자식 노드)
				Queue.add(w);
		}
	}
```

<br>

## 그래프 탐색

<br>

```java
BFS(G, v) { // 그래프 G, 탐색 시작점 v
		큐 생성
		시작점 v를 큐에 삽입
		점 v를 방문한 것으로 표시
		while 큐가 비어있지 않은 경우
			t <- 큐의 첫 번째 원소 반환
			for t와 연결된 모든 선에 대해
				u <- t의 이웃점
				u가 방문되지 않은 곳이면,
				u를 큐에 넣고 방문한 것으로 표시
	}
```

<br>

## 미로 탈출 길이

- 최단 길이 구하는 방법
    1. 2차원 배열을 만들어 직접 길이를 저장한다.
    2. 큐에 넣을 때 같이 넣어 저장한다. (클래스 멤버 변수로 함께 저장)
    3. 길이를 저장하는 변수를 생성하여 이를 활용한다. (Queue size를 묶어 같은 레벨끼리 처리)