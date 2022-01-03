---
title: '[Algorithm] Bronze 1-2. Rectangle Geometry'
date: 2021-11-22 22:30
category: 'Algorithm'
draft: false
---

**<u>공부 자료</u>** [(링크)](https://usaco.guide/bronze/time-comp?lang=cpp)

## 1. 개요

직사각형이 나오는 기하 문제는 가장 기초적인 형태의 기하 문제입니다. 직사각형은 모든 변이 서로 수직이기 때문에 좌표축과 격자점을 이용하여 나타내기 편리하기 때문입니다.

직사각형의 성질로 인해 왼쪽 하단 점의 좌표와 오른쪽 상단 점의 좌표만 주어지면 사각형이 특정된다는 점도 중요한 성질입니다.

## 2. 예제

[Example-Fence Painting](http://www.usaco.org/index.php?page=viewproblem2&cpid=567)

[백준에서 채점 가능](https://www.acmicpc.net/problem/11970)

직사각형 문제에 들어가기 전, 수직선 위의 두 선분의 교차하는 길이를 찾는 간단한 문제입니다.

풀이는 다음과 같습니다.

```c++
#include<iostream>
#include<algorithm>
using namespace std;
int main(){
    int a,b,c,d;
    cin >> a >> b >> c >> d;

    int total = b-a +d-c;
    int intersection = max(0, min(b,d) - max(a,c));
    int ans = total - intersection;
    cout << ans;
}
```

우선 두 선분의 길이를 구하여 더한 뒤, 거기서 겹치는 부분의 길이를 구해 빼주어 답을 구합니다.

여기서 겹치는 부분의 길이를 구할 때, 두 선분의 끝점 중 작은 값에서 두 선분의 시작점 중 큰 값을 빼서 구하고, 이 값이 0보다 작을 경우에는 겹치는 부분이 없다고 놓는 방법을 사용합니다.

[Example-Blocked Billboard](http://www.usaco.org/index.php?page=viewproblem2&cpid=759)

[백준에서 채점 가능](https://www.acmicpc.net/problem/15463)

앞의 문제를 응용하여, 이번에는 두 판의 겹치지 않는 부분의 넓이를 구하는 문제입니다.

풀이는 다음과 같습니다.

```c++
#include<iostream>
#include<algorithm>
using namespace std;

//structure of a rectangle
// (x1,y1) and (x2,y2) are the lower-left and upper-right coordinates
// function area() returns the area of a rectangle
struct Rect {
    int x1, x2, y1, y2;
    int area() {
        return (y2-y1) * (x2-x1);
    }
};

//function that calculates the intersected part of two rectangles and returns the area of it
int intersect (Rect p, Rect q){
    int xoverlap= max(0,min(p.x2,q.x2) - max(p.x1, q.x1));
    int yoverlap= max(0, min(p.y2, q.y2) - max(p.y1, q.y1));
    return xoverlap*yoverlap;
}

int main(){
    Rect board1, board2 ,truck;
    cin >> board1.x1 >> board1.y1 >> board1.x2 >> board1.y2;
    cin >> board2.x1 >> board2.y1 >> board2.x2 >> board2.y2;
    cin >> truck.x1 >> truck.y1 >> truck.x2 >> truck.y2;
    int ans=board1.area()+board2.area()-intersect(board1, truck) - intersect(board2, truck);
    cout << ans;
}
```

구조체 `Rect`를 이용하여 사각형을 왼쪽 하단 꼭짓점과 오른쪽 상단 꼭짓점을 이용하여 정의하고, 함수 `area()` 를 통해 사각형의 넓이를 리턴하도록 합니다.

위의 문제에서 선분의 겹치는 길이를 구한 것과 같은 방법으로 가로변과 세로변의 겹치는 길이를 구한 후, 이를 이용하여 겹치는 부분의 넓이를 구하여 전체 사각형의 넓이에서 빼주어 넓이를 구할 수 있습니다.

## 3. 자주 쓰이는 테크닉

- 변의 길이와 넓이

변의 길이는 간단하게 두 좌표 중 큰 것에서 작은 것을 빼서 구할 수 있으며, 그렇게 구한 가로와 세로의 길이를 곱하여 넓이를 구할 수 있습니다.

```c++
void rect(int a_x,int a_y,int b_x,int b_y){
    int width= b_x - a_x;
    int height=b_y - a_y;
    int area=width*height;
}
```

- 겹치는 부분

겹치는 부분은 두 선분의 끝점 중 작은 것에서, 두 선분의 시작점 중 큰 것을 빼서 구합니다. 이 값이 0보다 작다는 것은 겹치는 부분이 없다는 의미이므로 대신 0을 사용합니다.

```c++
int intersect(int a_start, int a_end, int b_start, int b_end){
    return max(0,min(a_end,b_end)-max(a_start,b_start));
}
```

## 4. 연습문제

[Square Pasture](http://www.usaco.org/index.php?page=viewproblem2&cpid=663)

[백준에서 채점 가능](https://www.acmicpc.net/problem/14173)

주어진 두 직사각형을 모두 덮는 최소 정사각형의 넓이를 구하며 되는 간단한 문제입니다.

풀이는 다음과 같습니다.

```c++
#include<iostream>
#include<algorithm>
using namespace std;
int main(){

    int x1,x2,y1,y2;
    cin >> x1 >> y1 >> x2 >> y2;

    int a1,a2,b1,b2;
    cin >> a1 >> b1 >> a2 >> b2;

    int min_x = min(x1,a1);
    int max_x = max(x2,a2);
    int min_y = min(y1,b1);
    int max_y = max(y2,b2);

    int side= max(max_x - min_x,max_y - min_y);
    int ans= side*side;
    cout << ans;
}
```

두 직사각형의 영역을 모두 포함시키는 폭과 높이를 계산한 뒤 더 큰 것을 한 변으로 하는 정사각형의 넓이를 구해주면 됩니다.

[Blocked Billboard 2](http://www.usaco.org/index.php?page=viewproblem2&cpid=783)

[백준에서 채점 가능](https://www.acmicpc.net/problem/15592)

첫번쨰 판에서 두번째 판에 가리지 않는 부분을 덮을 수 있는 최소 직사각형의 크기를 구하는 문제입니다.

풀이는 다음과 같습니다.

```c++
#include<iostream>
#include<algorithm>
using namespace std;
int main(){
    int x1,x2,y1,y2;
    cin>> x1 >> y1 >> x2 >> y2;
    int x3,x4,y3,y4;
    cin >> x3 >> y3 >> x4 >> y4;

    //Case 1: the second board fully covers the first board
    if (x3 <= x1 && y3 <= y1 && x2 <= x4 && y2 <= y4)
        cout << 0;
    //Case 2: the second board covers the bottom of the first board
    else if (x3 <= x1 && x2 <= x4 && y3 <= y1 && y4 <= y2)
        cout << (x2-x1)*(y2-y4);
    //Case 3: the second board covers the top of the first board
    else if (x3 <= x1 && y1 <= y3 && x2 <= x4 && y2 <= y4)
        cout << (x2-x1)*(y3-y1);
    //Case 4: the second board covers the left of the first board
    else if (x3 <= x1 && y3 <= y1 && x4 <= x2 && y2 <= y4)
        cout << (x2-x4)*(y2-y1);
    //Case 5: the second board covers the right of the first board
    else if (x1 <= x3 && y3 <= y1 && x2 <= x4 && y2 <= y4)
        cout << (x3-x1)*(y2-y1);
    //Case 6: Other cases(Covering the corner, no intersection)
    else
        cout << (x2-x1)*(y2-y1);
}
```

첫번째 판과 두번쨰 판의 배치에 따라 6가지 경우로 나누어 주어 필요한 정사각형의 크기를 계산해 주면 됩니다.

[White Sheet](https://codeforces.com/contest/1216/problem/C)

흰 판에서 두 검은 판에 포함되지 않는 부분의 유무를 구하는 문제입니다.

풀이는 다음과 같습니다.

```c++
#include<iostream>
#include<algorithm>
using namespace std;

struct Rect {
    long long x1, x2, y1, y2;
    long long area() {
        return (y2-y1) * (x2-x1);
    }
};

long long intersect (Rect p, Rect q){
    long long xoverlap= max((long long) 0, min(p.x2,q.x2) - max(p.x1, q.x1));
    long long yoverlap= max((long long) 0, min(p.y2, q.y2) - max(p.y1, q.y1));
    return xoverlap*yoverlap;
}

long long intersect_three (Rect p, Rect q, Rect r){
    long long xoverlap= max((long long)0, min(min(p.x2,q.x2),r.x2) - max(max(p.x1, q.x1),r.x1));
    long long yoverlap= max((long long)0, min(min(p.y2, q.y2),r.y2) - max(max(p.y1, q.y1),r.y1));
    return xoverlap*yoverlap;
}

int main(){
    Rect w, b1, b2;
    cin >> w.x1 >> w.y1 >> w.x2 >> w.y2;
    cin >> b1.x1 >> b1.y1 >> b1.x2 >> b1.y2;
    cin >> b2.x1 >> b2.y1 >> b2.x2 >> b2.y2;

    long long visible=w.area()-intersect(w,b1) -intersect(w,b2)+intersect_three(w,b1,b2);
    if(visible >0)
        cout << "YES";
    else
        cout << "NO";
}
```

연습문제를 응용하여 세 직사각형에 모두 포함되는 영역을 구해줄 수 있습니다.

흰 직사각형의 넓이 - 검은 직사각형으로 덮인 부분의 넓이 + 두 검은 직사각형에 모두 덮인 부분의 넓이 를 계산하여 그 값이 0보다 큰지를 통해 덮이지 않은 부분의 유무를 판별할 수 있습니다.

[Two Tables](https://codeforces.com/problemset/problem/1555/B)

두 책상이 모두 방 안에 배치될 수 있도록 첫 사각형을 밀어야 할 최소 거리를 구하는 문제입니다.

풀이는 다음과 같습니다.

```c++
#include<iostream>
#include<algorithm>
using namespace std;
int main(){
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
    int k;
    cin >> k;
    while(k--){
        int max_w, max_h;
        cin >> max_w >> max_h;
        int x1, y1, x2, y2;
        cin >> x1 >> y1 >> x2 >> y2;
        int width=x2-x1;
        int height=y2-y1;
        int w, h;
        cin >> w >> h;


        int move_x=-1, move_y=-1;
        if(width+w <= max_w)
            move_x = max(0, w-max(x1,max_w-x2));
        if(height + h <= max_h)
            move_y = max(0, h-max(y1,max_h-y2));

        if(move_x ==-1 && move_y !=-1)
            cout << move_y << '\n';
        else if (move_x !=-1 && move_y == -1)
            cout << move_x << '\n';
        else if (move_x != -1 && move_y !=-1)
            cout << min(move_x, move_y) << '\n';
        else
            cout << -1 << '\n';
    }
}
```

x와 y 방향으로 이동해야할 필요성과 이동해야 할 거리를 각각 구한 다음, 둘 중 최소로 이동하면 됩니다.
