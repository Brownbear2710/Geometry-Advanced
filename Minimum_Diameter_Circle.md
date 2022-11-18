# [Problem](https://vjudge.net/problem/SPOJ-QCJ4)
Given n points in a plane find the diameter of the smallest circle that encloses all the points. A point lying on the circle is also considered to be inside it.<br>
<details>
<summary>Hint</summary>
Ternary search.
</details>

<details>
<summary>Solution</summary>
Let's say we know on which straight line, parallel to the x-axis, the center of the smallest enclosing circle exists. Now staying on that line, if we move to the left, we increase the distances of the points  from the center that are to the right and vice versa. So, on a straight line parallel to the x-axis, the ternary property exists.<br><br>
<img src= "https://github.com/Brownbear2710/Geometry-Advanced/blob/main/images/qcj4_1.jpg" alt="Fig.1a" style="height: 500px; width:500px;"/> <br><br>
Now, if we know on which straight line, parallel to the y-axis, the center of the smallest enclosing circle exists, we can also see that the ternary property along the y-axis exists.<br><br>
<img src= "https://github.com/Brownbear2710/Geometry-Advanced/blob/main/images/qcj4_2.jpg" alt="Fig.1b" style="height: 500px; width:500px;"/> <br><br>
So, now we can apply nested ternary search along both axes to find the smallest diameter of the circle that can cover all the points.<br><br>
<img src= "https://github.com/Brownbear2710/Geometry-Advanced/blob/main/images/qcj4_3.jpg" alt="Fig.1b" style="height: 500px; width:500px;"/> <br><br>

</details>

<details>
<summary>C++ Code</summary>

```cpp
#include <bits/stdc++.h>

using namespace std;
using ld = long double;

struct point
{
    ld x, y;
};

ld dist2(point p1, point p2)
{
    return (p1.x - p2.x) * (p1.x - p2.x) + (p1.y - p2.y) * (p1.y - p2.y);
}

// For a fixed center (cx,cy) find the radius of the circle that
// can cover all the points
ld best(vector<point> &v, ld cx, ld cy)
{
    ld r2 = -1;
    for(int i = 0; i < v.size(); i++)
    {
        r2 = max(r2, dist2(v[i], {cx,cy}));
    }
    return r2;
}

// Ternary search along y-axis
ld search_y(vector<point> &v, ld cx)
{
    ld lo = 0, hi = 1001;
    for(int i = 0; i < 50; i++)
    {
        ld m1 = (lo*2 + hi)/3;
        ld m2 = (lo + hi*2)/3;
        ld r1 = best(v, cx, m1);
        ld r2 = best(v, cx, m2);
        if(r1 < r2) hi = m2;
        else lo = m1;
    }
    return best(v, cx, lo);
}

// Ternary search along x-axis
ld search_x(vector<point> &v)
{
    ld lo = 0, hi = 1001;
    for(int i = 0; i < 50; i++)
    {
        ld m1 = (lo*2 + hi)/3;
        ld m2 = (lo + hi*2)/3;
        ld r1 = search_y(v, m1);
        ld r2 = search_y(v, m2);
        if(r1 < r2) hi = m2;
        else lo = m1; 
    }
    return search_y(v,lo);
}

int main()
{
    ios_base::sync_with_stdio(0);cin.tie(NULL);
    int n;
    cin >> n;
    vector<point> v(n);
    for(auto &p : v)
        cin >> p.x >> p.y;
    double d = search_x(v);
    d = 2*sqrt(d);
    cout << fixed << setprecision(2) << d << "\n";
    return 0;
}
```
