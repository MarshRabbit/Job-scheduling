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
