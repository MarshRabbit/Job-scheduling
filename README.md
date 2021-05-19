# Job-scheduling

## 작업 스케쥴링

**작업 스케줄링 문제** 는 n개의 작업, 각 작업의 수행 시간 t<sub>i</sub>(i=1,2,3,4,5,...n) 그리고 m개의 동일한 기계가 주어질 때, 모든 작업이 가장 빨리 종료되도록 작업을 기계(machine)에 배정하는 문제이다.

작업 스케줄링 문제를 해결하기 위한 가장 간단한 방법은 그리디 알고리즘을 이용해 푸는 것이다.
즉, 배정된 작업이 가장 빨리 끝나는 기계에 새 작업을 배정해주는 것이다.

## Greedy 설계 과정

기계가 2대로 가정하므로 m = 2

1. 기계의 마지막 작업시간을 넣을 배열을 0으로 초기화한다
2. 배열안의 내용을 비교해서 더 일찍 끝나는 기계를 찾는다
3. 더 일찍 끝나는 기계에 t<sub>i</sub>만큼 작업 시간을 추가한다
4. 가장 늦은 작업 종료 시간을 return한다

## 자바 코드
```java
public class Scheduling {
    private void greedy() {
        int x = 2;
        while (x<6) {
            int[] machine = {0, 0};
            int num = x; // 작업의 개수
            int min; // 가장 일찍 끝나는 기계 번호

            int[] n = new int[num]; //작업의 시간을 넣을 배열
            System.out.println("작업의 개수: "+ x);
            System.out.print("작업의 시간은 ");
            for (int i = 0; i < num; i++) {
                n[i] = (int) (Math.random() * 10) + 1;
                System.out.print(n[i] + " ");
            }
            System.out.println();

            for (int i = 0; i < num; i++) { //작업 배정 반복문
                min = 0;
                if (machine[1] < machine[0])
                    min = 1;

                machine[min] += n[i];
            }

            System.out.println(machine[0] + " " + machine[1]+ "\n");
            x++;
        }
    }
    public static void main(String[] args) {
        Scheduling scheduling = new Scheduling();
        scheduling.greedy();
    }
}
```

<img width="293" alt="스크린샷 2021-05-19 오후 9 35 29" src="https://user-images.githubusercontent.com/80511335/118813542-37071080-b8ea-11eb-9bda-6b8cd7b85bd9.png">

이때 시간복잡도는 작업시간을 추가할때마다 기계를 비교해서 마지막 작업시간이 적은곳 에다가 추가해야 하므로 기계를 비교하는 횟수 O(M) x 작업의 수 O(N) 이므로 O(MN)이 된다.

## 손으로 구한 최적해

![IMG_57FDCC215A20-1](https://user-images.githubusercontent.com/80511335/118815158-e85a7600-b8eb-11eb-96da-d0c508f7bb57.jpeg)

## 근사 비율

최적해를 OPT, 근사해를 OPT' 이라고 하고 작업 i가 시간 T 직후에 배정된다 하면 모든 작업이 T + t<sub>i</sub> 에 종료된다.

그러므로 OPT' = T + t<sub>i</sub> 이고 T'가 작업 i를 제외한 평균 종료시간이면  T <= T'이 된다. 이 관계를 OPT'와 OPT로 나타내면

![118807825-bc3af700-b8e3-11eb-814c-fb66d0ff89f6](https://user-images.githubusercontent.com/80511335/118817982-c8788180-b8ee-11eb-91bf-3b286cadd1a9.jpg)

결국 OPT' <= 2OPT로 근사해는 최적해의 두배를 넘지 않는다


## Brute force
```java
public class Scheduling {
    static int[] soda = new int[500000];
    static int z;

    public static void combi(int[] arr, boolean[] visited, int depth, int r) {
        if (r == 0) {
            sum(arr, visited);
            return;
        }
        if (depth == arr.length) return;

        visited[depth] = true;
        combi(arr, visited, depth + 1, r -1);
        visited[depth] = false;
        combi(arr, visited, depth + 1, r);
    }

    public static void sum(int[] arr, boolean[] visited) {
        int[] sum = {0, 0};
        for (int i = 0; i < arr.length; i++) {
            if (visited[i])
                sum[0] += arr[i];
            else if (!visited[i])
                sum[1] += arr[i];
        }
        soda[z++] = Math.max(sum[0], sum[1]);
    }

    public static void main(String[] args) {
        boolean[] visited_4 = {false, false, false, false};
        boolean[] visited_8 = {false, false, false, false, false, false, false, false};
        boolean[] visited_16 = {false, false, false, false, false, false, false, false, false,
                false, false, false, false, false, false, false};
        int x = 4;

        while (x <= 16) {
            int[] machine = {0, 0};
            int num = x; // 작업의 개수
            int min; // 가장 일찍 끝나는 기계 번호

            int[] t = new int[num]; //작업의 시간을 넣을 배열
            System.out.println("작업의 개수: "+ x);
            System.out.print("작업의 시간은 ");
            for (int i = 0; i < num; i++) {
                t[i] = (int) (Math.random() * 10) + 1;
                System.out.print(t[i] + " ");
            }
            System.out.println();

            for (int i = 0; i < num; i++) { //작업 배정 반복문
                min = 0;
                if (machine[1] < machine[0])
                    min = 1;

                machine[min] += t[i];
            }

            System.out.println("근사해는 " + Math.max(machine[0], machine[1]));

            z = 0;
            if (x == 4) {
                for (int i = 1; i <= num; i++)
                    combi(t, visited_4, 0, i);
            }
            else if (x == 8) {
                for (int i = 1; i <= num; i++)
                    combi(t, visited_8, 0, i);
            }
            else if (x == 16) {
                for (int i = 1; i <= num; i++)
                    combi(t, visited_16, 0, i);
            }

            int mini = 99999999;
            for (int i = 0; i < Math.pow(2, x)-1; i++) {
                if (soda[i] < mini)
                    mini = soda[i];
            }

            System.out.println("최적해는 " + mini + "\n");

            x = x*2;
        }

    }
}
```
<img width="382" alt="스크린샷 2021-05-20 오전 12 27 42" src="https://user-images.githubusercontent.com/80511335/118840252-324e5680-b902-11eb-9518-b74782ed4ad6.png">
