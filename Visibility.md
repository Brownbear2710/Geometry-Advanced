# Problem
Give a polygon and a point inside the polygon, you have to say if a person standing at that point can see the whole interior of that polygon. A person can see a point if there is nothing in between the person and the point.<br>


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