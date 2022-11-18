# [Minimum Diameter Circle]((https://vjudge.net/problem/SPOJ-QCJ4))

<details>
<summary>Problem</summary>
Given n points in a plane find the diameter of the smallest circle that encloses all the points. A point lying on the circle is also considered to be inside it.
</details>

<details>
<summary>Hint</summary>
Ternary search.
</details>

<details>
<summary>Solution</summary>
Let's say we know on which straight line, parallel to x-axis, the center of the circle exists. Now staying on that line, if we move to the left, we increase the distance that are to right and vice versa. So, on a straight line parallel to x-axis, the ternary property exists.

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
</details>
<br>
<details>
<summary>Problem</summary>
Give a polygon and a point inside the polygon, you have to say if a person standing at that point can see the whole interior of that polygon. A person can see a point if there is nothing in between the person and the point.
</details>

<details>
<summary>Hint</summary>
</details>

<details>
<summary>Solution</summary>
</details>

<details>
<summary>C++ Code</summary>

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

struct point
{
    ll x, y;
};

ll delta(point a, point b, point c)
{
    return (a.x - b.x)*(b.y - c.y) - (a.y - b.y)*(b.x - c.x);
}

int main()
{
    ios_base::sync_with_stdio(0);cin.tie(NULL);
    int n;
    cin >> n;
    vector<point> v(n);
    for(auto  & p : v)
        cin >> p.x >> p.y;
    point reff;
    cin >> reff.x >> reff.y;
    int sign = (delta(v[0], v[1], reff) >= 0 ? 1 : -1);
    bool f = true;
    for(int i = 0; i < n; i++)
        if(sign * delta(v[i], v[(i+1)%n], reff) < 0)
            f = false;
    cout << (f ? "YES\n" : "NO\n");
    return 0;
}
```
</details>