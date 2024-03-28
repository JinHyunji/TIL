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

## 깊이 우선 탐색 (Depth First Search, DFS)

### DFS 란?

- 깊이 우선 탐색
- 시작 지점에서 출발하여 한 방향으로 탐색함
- 진행할 수 없다면 마지막에 만난 지점으로 돌아와 다른 방향 다시 탐색
- 후입선출(LIFO : Last-In First-Out) 구조의 스택 사용
- 재귀함수는 System Stack을 활용하므로 간단하게 구현 가능

<br>

### 트리 탐색

- while 문
1. 루트 노드 → Stack Push
2. Stack → Empty 가 될 때까지 반복 수행
    1. 현재 노드 ← Stack pop
    2. 현재 노드의 모든 자식 → Stack Push

<br>

```code
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

- 재귀 함수
    1. 현재 노드(V) 방문
    2. V의 자식 노드(W)를 차례로 재귀 호출

<br>

```code
// v : 루트 노드
DFS(v) {
	v 방문;
	for w in (v의 모든 자식) {
		DFS(w);
	}
}
```

<br>

### 그래프 탐색 (재귀 함수)

<br>

```code
// G : 그래프, visited : 방문 배열
DFS(v) {
	visited[v] <- true // v 방문 설정
	
	for each all w in adjacency(G, v);
		if visited[w] != true
			DFS(w)
}
```


<br>

### 2차원 배열 탐색 방법 (재귀 함수)

<br>

```Code
// arr : 2차원 배열, visited : 방문 배열
DFS(r, c) {
	visited[r][c] <- true // v 방문 설정
	for (r, c)를 기준으로 4방향 탐색
		if 다음 좌표는 이동 가능한 것인지 체크
			DFS(nr, nc)
}
```

<br>

```Java
import java.util.Scanner;

public class 그래프탐색_01_DFS {

	static int V; // 정점의 수
	static int[][] adj; // 인접 행렬
	static boolean[] visited; // 방문 체크

	public static void main(String[] args) {
		Scanner sc = new Scanner(input);

		V = sc.nextInt();
		int E = sc.nextInt();

		adj = new int[V + 1][V + 1]; // 시작 정점이 1번부터
		visited = new boolean[V + 1];

		for (int i = 0; i < E; i++) {
			int A = sc.nextInt();
			int B = sc.nextInt();
			adj[A][B] = adj[B][A] = 1; // 인접 행렬 (무향)
		} // 간선 정보 입력 완료

		DFS(1);
	}

	public static void DFS(int v) {
		// V 방문 처리
		visited[v] = true;
		System.out.println(v);

		// 인접한 친구들 방문 (인접 행렬, 인접 리스트 코드가 조금 다름)
		for (int i = 1; i <= V; i++) {
			if (!visited[i] && adj[v][i] == 1) {
				DFS(i);
			}
		}

		// 인접 리스트 사용 시
//		for (int w: adj[V]) {
//			if (!visited[w]) {
//				DFS(W);
//			}
//		}
	}

	static String input = "7 9 \r\n" + "1 2\r\n" + "1 3\r\n" + "1 6 \r\n" + "2 4 \r\n" + "2 7 \r\n" + "3 4 \r\n"
			+ "4 7 \r\n" + "5 6 \r\n" + "5 7 \r\n";

}
```

<br>


## 너비 우선 탐색 (Breadth First Search, BFS)

### BFS 란?

- 너비 우선 탐색
- 시작 지점에 인접한 순으로 탐색을 시작함
- 인접한 지점을 모두 방문하였다면 다음으로 인접한 지점을 방문함
- 선입선출(FIFO : First-In First-Out) 구조의 Queue 자료구조를 사용
- 너비 우선 탐색은 인접한 지점부터 방문을 하므로 시작 지점과 끝 지점이 주어졌을 때 최단 길이를 구할 수 있음

<br>

### 트리 탐색

1. 루트 노드 Queue에 삽입
2. Queue가 공백이 될 때까지 반복 수행
    1. Queue에서 원소(Curr) 꺼내기
    2. 해당 원소 방문
    3. Curr의 자식 노드 Queue에 삽입

<br>

```code
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

### 그래프 탐색

<br>

```code
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

```Java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.Scanner;

public class 그래프탐색_02_BFS {

	static int V; // 정점의 수
	static List<Integer>[] adj;
	static boolean[] visited; // 방문 체크

	public static void main(String[] args) {
		Scanner sc = new Scanner(input);

		V = sc.nextInt();
		int E = sc.nextInt();
		adj = new ArrayList[V+1];
		for (int i = 1; i <= V; i++) {
			adj[i] = new ArrayList<>();
		}
		visited = new boolean[V + 1];

		for (int i = 0; i < E; i++) {
			int A = sc.nextInt();
			int B = sc.nextInt();
			
			adj[A].add(B);
			adj[B].add(A); // 무향이니까
			
		} // 간선 정보 입력 완료
		
		BFS(1);
	}
	
	// v : 시작 정점
	public static void BFS(int v) {
		Queue<Integer> queue = new LinkedList<>();
		
		queue.add(v); // 시작 정점을 큐에 넣는다.
		visited[v] = true; // 시작 정점 방문 처리한다.
		
		// 큐가 공백 상태가 될 때까지 반복문 수행
		while (!queue.isEmpty()) {
			int curr = queue.poll(); // 정점 하나를 꺼내!
			System.out.println(curr); // 경로 한 번 찍어보기
			
			// 인접 리스트
			for (int w : adj[curr]) {
				if (!visited[w]) {
					queue.add(w);
					visited[w] = true; // 미리 방문 처리를 해서 중복으로 큐에 넣는 것 방지
				}
			}
		}
		
	}
	
	static String input = "7 9 \r\n" + "1 2\r\n" + "1 3\r\n" + "1 6 \r\n" + "2 4 \r\n" + "2 7 \r\n" + "3 4 \r\n"
			+ "4 7 \r\n" + "5 6 \r\n" + "5 7 \r\n";
}
```

<br>


### 미로 탈출 길이

- 최단 길이 구하는 방법
    1. 2차원 배열을 만들어 직접 길이를 저장한다.
    2. 큐에 넣을 때 같이 넣어 저장한다. (클래스 멤버 변수로 함께 저장)
    3. 길이를 저장하는 변수를 생성하여 이를 활용한다. (Queue size를 묶어 같은 레벨끼리 처리)


<br>
<br>
<br>
<br>


# 4. 그래프 비용

## 서로소 집합 (Disjoint-sets)

### 상호 배타 집합

- 중복 포함된 원소가 없는 집합 → 교집합이 없음
- 각 집합은 대표자를 통해 구분

<br>

### 상호 배타 집합 표현 방법

- 연결 리스트
- 트리

<br>

### 상호 배타 집합 연산

- Make-Set(x)
    - 유일한 멤버 x를 포함하는 새로운 집합을 생성하는 연산
    - 반복문을 이용하여 간소화

- Fine-Set(x)
    - x를 포함하는 집합을 찾는 연산
    - 특정 노드에서 루트 노드까지의 경로를 찾아 가면서 노드 부모의 정보를 갱신

- Union(x, y) : x와 y를 포함하는 두 집합을 통합하는 연산

- 연산의 효율을 높이는 방법
    - Rank를 이용한 Union
        - 각 노드는 자신을 루트로 하는 subtree의 높이를 랭크(rank)라는 이름으로 저장
        - 두 집합을 Union할 때 rank가 낮은 집합을 높은 집합에 붙인다.
        - 두 대표자의 rank가 다를 경우 : rank가 높은 쪽으로 붙이기
        - 두 대표자의 rank가 같을 경우 : 아무 쪽이나 붙여도 된다.
    - Path Compression
        - Find-Set을 행하는 과정에서 만나는 모든 노드들이 직접 대표를 가리키도록 수정한다.

<br>


### 상호 배타 집합 표현 - 연결 리스트

- 같은 집합의 원소들은 하나의 연결리스트로 관리
- 연결리스트의 맨 앞의 원소를 집합의 대표자로 결정
- 각 원소는 집합의 대표 원소를 가리키는 링크를 갖는다.

<br>


### 상호 배타 집합 표현 - 트리

- 하나의 집합을 하나의 트리로 표현한다.
- 자식 노드가 부모 노드를 가리키며 루트 노드가 대표자가 된다.

<br>


## 최소 비용 신장 트리 (Minimum Spanning Tree, MST)

### 신장 트리

- 그래프의 모든 정점과 간선의 부분 집합으로 구성되는 트리

<br>

### 최소 신장 트리

- 신장 트리 중에서 사용된 간선들의 가중치 합이 최소인 트리
- 무 방향 가중치 그래프
- N개의 정점을 가지는 그래프에 대해 반드시 (N-1)개의 간선을 사용
- 사이클을 포함 X

<br>

### 왜 사용하는가?

- 도로망, 통신망, 유통망 등 여러 분야에서 비용을 최소로 해야 이익을 볼 수 있다.
- 대표적인 알고리즘으로 크루스칼, 프림이 있음

<br>

## 크루스칼 알고리즘

### 크루스칼 알고리즘

1. 최초 모든 간선을 가중치에 따라 오름차순으로 정렬
2. 가중치가 가장 낮은 간선부터 선택하면서 트리를 증가시킴 → 사이클이 존재하면 다음으로 가중치가 낮은 간선 선택
3. N-1개의 간선이 선택될 때까지 2 반복

<br>

### 크루스칼 알고리즘 의사코드

```Code
MST-KRUSKAL(G)
	A <- 0			// 0 : 공집합
	FOR v in G.V	// G.V : 그래프의 정점 집합
		Make-Set(v)	// G.E : 그래프의 간선 집합
		
	G.E에 포함된 간선들을 가중치 w에 의해 정렬
		
	FOR 가중치가 가장 낮은 간선 (u, v) ∈ G.E 선택(n-1개)
		IF Find-Set(u) ≠ Find-Set(V)
			A <- A ∪ {(u, v)}
			Union(u, v);
	RETURN A
```

<br>

## 프림 알고리즘

### Prim 알고리즘

- 하나의 정점에서 연결된 간선들 중에 하나씩 선택하면서 MST를 만들어가는 방식
1. 임의 정점을 선택하여 시작
2. 선택한 정점과 인접하는 정점들 중의 최소 비용의 간선이 존재하는 정점을 선택
3. 모든 정점이 선택될 때까지 2. 과정을 반복
- 서로소인 2개의 집합 정보를 유지
    - 트리 정점 : MST를 만들기 위해 선택된 정점들
    - 비트리 정점들 : 선택되지 않은 정점들

<br>

### 프림 알고리즘 코드

<br>

- 반복문으로 구현

```java
import java.util.Arrays;
import java.util.Scanner;

public class 프림_반복문 {

	static final int INF = Integer.MAX_VALUE; // 이주 큰 값으로 초기화
	
	public static void main(String[] args) {
		Scanner sc = new Scanner(input);
		
		int V = sc.nextInt(); // 0부터 시작
		int E = sc.nextInt(); // 간선의 개수
		
		// 인접 행렬
		int[][] adjArr = new int[V][V];
		
		for (int i = 0; i < E; i++) {
			int A = sc.nextInt();
			int B = sc.nextInt();
			int W = sc.nextInt();
			
			// 무향 그래프
			adjArr[A][B] = adjArr[B][A] = W;
		} // 입력 끝
		
		// 방문 처리를 위해 배열 선언
		boolean[] visited = new boolean[V];
		int[] p = new int[V]; // 내가 어디서 왔는지 확인 -> 문제에 따라 필요 유무 다름
		int[] dist = new int[V]; // 가장 적은 비용을 저장하기 위한 배열
		
		// dist 초기화
		for (int i = 0; i < V; i++) {
			dist[i] = INF;
			p[i] = -1;
		}
//		Arrays.fill(dist, INF); // 위의 반복문과 같은 기능을 하는 메서드
		
		// 임의의 한 점을 선택해서 돌려야 함
		dist[0] = 0; // 0번 정점부터 시작
		
		int ans = 0;
		
		// 정점을 선택하는 사이클 :
		// 정점의 개수만큼 반복해도 상관없지만
		// 성능 향상을 위해 V-1 (가지치기)
		for (int i = 0; i < V; i++) {
			int min = INF;
			int idx = -1;
			
			// 아직 안 뽑힌 정점들 중 가장 작은 값을 뽑음
			for ( int j = 0; j < V; j++) {
				if (!visited[j] && dist[j] < min) {
					min = dist[j];
					idx = j; 
				}
			} // 해당 반복문 종료 시 idx는 가장 작은 값을 가지고 있고 방문하지 않은 정점이 선택됨
			
			visited[idx] = true; // 선택한 정점은 방문 처리
			
			// 선택한 정점과 인접한 정점들 중 갱신할 수 있으면 갱신
			for (int j = 0; j < V; j++) {
				if (!visited[j] && adjArr[idx][j] != 0 && dist[j] > adjArr[idx][j]) {
					dist[j] = adjArr[idx][j];
					p[j] = idx;
				}
			}
		}
		
		for (int i = 0; i < V; i++) {
			ans += dist[i];
		}
		
		System.out.println(Arrays.toString(dist));
		System.out.println(Arrays.toString(p));
		System.out.println(ans);
	}
	
	static String input = "7 11\r\n" + "0 1 32\r\n" + "0 2 31\r \n " + "0 5 60\r\n" + "0 6 51\r\n" + "1 2 21\r\n"
			+ "2 4 46\r\n" + "2 6 25\r\n" + "3 4 34\r\n" + "3 5 18\r\n" + "4 5 40\r\n" + "4 6 51\r\n" + "";
}
```

<br>

- 우선순위 큐로 구현

```java
import java.util.ArrayList;
import java.util.List;
import java.util.PriorityQueue;
import java.util.Scanner;

public class 프림_우선순위큐 {

	static final int INF = Integer.MAX_VALUE; // 이주 큰 값으로 초기화
	
	static class Edge implements Comparable<Edge> {
		int st, ed, w; // 시작과 끝 노드 (가중치도 추가 가능)

		public Edge(int st, int ed, int w) {
			this.st = st;
			this.ed = ed;
			this.w = w;
		}

		@Override
		public int compareTo(Edge o) {
			return Integer.compare(this.w,  o.w);
		}		
	}
	
	public static void main(String[] args) {
		Scanner sc = new Scanner(input);
		
		int V = sc.nextInt(); // 0부터 시작
		int E = sc.nextInt(); // 간선의 개수
		
		// 인접 행렬
		List<Edge>[] adjList = new ArrayList[V];
		
		for (int i = 0; i < V; i++) {
			adjList[i] = new ArrayList<>();
		}
		
		for (int i = 0; i < E; i++) {
			int A = sc.nextInt();
			int B = sc.nextInt();
			int W = sc.nextInt();
			
			// 무향 그래프
			adjList[A].add(new Edge(A, B, W));
			adjList[B].add(new Edge(B, A, W));
			
		} // 입력 끝
		
		// 방문 처리를 위해 배열 선언
		boolean[] visited = new boolean[V];
		
		PriorityQueue<Edge> pq = new PriorityQueue<>();
		
		visited[0] = true; // 0번 정점은 시작
		
		// 0번 정점과 인접한 정점들을 전부 넣기
//		for (int i = 0; i < adjList[0].size(); i++) {
//			pq.add(adjList[0].get(i));
//		}
//		for (Edge e : adjList[0]) {
//			pq.add(e);
//		}
		pq.addAll(adjList[0]);
		
		int pick = 1; // 현재 확보한 정점의 개수
		int ans = 0; // 비용도 0
		
		while (pick != V) {
			Edge e = pq.poll();
			if (visited[e.ed]) continue; // 이미 해당 정점이 방문한 정점이라면
			
			ans += e.w; // 해당 간선이 가지고 있는 가중치를 더함
			visited[e.ed] = true;
			pick++;
			
			// 반복문을 돌면서 갱신할 수 있는거 전부 갱신
			pq.addAll(adjList[e.ed]);
		}
		
		System.out.println(ans);
	}
	
	static String input = "7 11\r\n" + "0 1 32\r\n" + "0 2 31\r \n " + "0 5 60\r\n" + "0 6 51\r\n" + "1 2 21\r\n"
			+ "2 4 46\r\n" + "2 6 25\r\n" + "3 4 34\r\n" + "3 5 18\r\n" + "4 5 40\r\n" + "4 6 51\r\n" + "";
}
```

<br>

## 최단 경로

### 최단 경로 정의

- 가중치가 있는 그래프에서 두 정점 사이의 경로들 중 간선의 가중치의 합이 최소인 경로

<br>

### 하나의 시작 정점에서 끝 정점까지의 최단 경로

- 다익스트라 알고리즘 (음의 가중치 허용 X)
- 벨만-포트 알고리즘 (음의 가중치 허용 O)

<br>

### 모든 정점들에 대한 최단 경로

- 플로이드-워셜 알고리즘

<br>

## Dijkstra 알고리즘

### 다익스트라 알고리즘

- 시작 정점에서 거리가 최소인 정점을 선택해 나가면서 최단 경로를 구하는 방식
- 탐욕 알고리즘 중 하나이고, 프림 알고리즘과 유사함
- 정점 A에서 정점 B까지의 최단 경로 (A → X + X → B)

<br>

### 다익스트라 알고리즘 동작 과정

1. 시작 정점 입력
2. 거리 저장 배열을 ∞로 초기화
3. 시작점에서 갈 수 있는 곳의 값 갱신
4. 아직 방문하지 않은 점들이 가지고 있는 거리 값과 현재 정점에서 방문하지 않은 정점까지의 가중치의 합이 작다면 갱신
5. 모든 정점을 방문할 때까지 반복

<br>

### 다익스트라 코드

<br>

- 반복문으로 구현

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;

public class 다익스트라_반복문 {

	static class Node {
		int v, w;

		public Node(int v, int w) {
			this.v = v;
			this.w = w;
		}

	}

	static final int INF = 987654321;
	static int V, E;
	static List<Node>[] adjList;
	static int[] dist;

	public static void main(String[] args) {
		Scanner sc = new Scanner(input);
		
		V = sc.nextInt();
		E = sc.nextInt();
		
		adjList = new ArrayList[V];
		
		for (int i = 0; i < V; i++) {
			adjList[i] = new ArrayList<>();
		}
		
		dist = new int[V];
		Arrays.fill(dist, INF);
		
		for (int i = 0; i < E; i++) {
			// 시작 정점, 도착 정점, 가중치 순으로 입력
			adjList[sc.nextInt()].add(new Node(sc.nextInt(), sc.nextInt()));
		}
		
		dijkstra(0);
		
		System.out.println(Arrays.toString(dist));
		
		
	}

	private static void dijkstra(int start) {
		boolean[] visited = new boolean[V]; // 방문처리
		
		dist[start] = 0; // 시작 노드까지의 거리는 0으로 초기화
		
		// 모든 정점을 다 돌 때까지 반복
		for (int i = 0; i < V-1; i++) {
			int min = INF;
			int idx = -1;
			
			for (int j = 0; j < V; j++) {
				if (!visited[j] && min > dist[j]) {
					min = dist[j];
					idx = j;
				}
			} // idx는 방문하지 않았으면서 시작 정점으로부터 해당 idx까지의 거리가 최소인 정점
			
			if (idx == -1) break; // 시작 정점으로부터 갈 수 있는 정점들은 다 방문했다는 뜻
			
			visited[idx] = true; // 선언
			
			// 아래 방법이 더 간단함
			for (Node node : adjList[idx]) {
				if (!visited[node.v] && dist[node.v] > dist[idx] + node.w) {
					dist[node.v] = dist[idx] + node.w;
				}
			}

		}
	}

	static String input = "6 11\r\n" + "0 1 4\r\n" + "0 2 2\r\n" + "0 5 25\r\n" + "1 3 8\r\n" + "1 4 7\r\n"
			+ "2 1 1\r\n" + "2 4 4\r\n" + "3 0 3\r\n" + "3 5 6\r\n" + "4 3 5\r\n" + "4 5 12\r\n" + "";
}
```

<br>

- 우선순위 큐로 구현

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.PriorityQueue;
import java.util.Scanner;

public class 다익스트라_우선순위큐 {

	static class Node implements Comparable<Node> {
		int v, w;

		public Node(int v, int w) {
			this.v = v;
			this.w = w;
		}

		@Override
		public int compareTo(Node o) {
			return Integer.compare(this.w, o.w);
		}

	}

	static final int INF = 987654321;
	static int V, E;
	static List<Node>[] adjList;
	static int[] dist;

	public static void main(String[] args) {
		Scanner sc = new Scanner(input);
		
		V = sc.nextInt();
		E = sc.nextInt();
		
		adjList = new ArrayList[V];
		
		for (int i = 0; i < V; i++) {
			adjList[i] = new ArrayList<>();
		}
		
		dist = new int[V];
		Arrays.fill(dist, INF);
		
		for (int i = 0; i < E; i++) {
			// 시작 정점, 도착 정점, 가중치 순으로 입력
			adjList[sc.nextInt()].add(new Node(sc.nextInt(), sc.nextInt()));
		}
		
		dijkstra(0);
		
		System.out.println(Arrays.toString(dist));
	}

	private static void dijkstra(int start) {
		PriorityQueue<Node> pq = new PriorityQueue<>();
		boolean[] visited = new boolean[V]; // 방문처리
		
		dist[start] = 0; // 시작 노드까지의 거리는 0으로 초기화

		pq.add(new Node(start, 0));

		while (!pq.isEmpty()) {
			Node curr = pq.poll();
			
			if (visited[curr.v]) continue; // 이미 방문했다면 비용을 알고 있다는 뜻
			visited[curr.v] = true; // 선택
			
			for (Node node : adjList[curr.v]) {
				if (!visited[node.v] && dist[node.v] > dist[curr.v] + node.w) {
					dist[node.v] = dist[curr.v] + node.w;
					pq.add(new Node(node.v, dist[node.v]));
				}
			}
		}
	}

	static String input = "6 11\r\n" + "0 1 4\r\n" + "0 2 2\r\n" + "0 5 25\r\n" + "1 3 8\r\n" + "1 4 7\r\n"
			+ "2 1 1\r\n" + "2 4 4\r\n" + "3 0 3\r\n" + "3 5 6\r\n" + "4 3 5\r\n" + "4 5 12\r\n" + "";
}
```