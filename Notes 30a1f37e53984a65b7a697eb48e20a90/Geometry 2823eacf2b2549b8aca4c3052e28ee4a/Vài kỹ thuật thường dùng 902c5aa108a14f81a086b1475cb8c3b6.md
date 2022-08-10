# Vài kỹ thuật thường dùng

# Nén vector

```cpp
point p;
if (p != make_pair(0, 0) {
	int d = __gcd(abs(p.first), abs(p.second));
	p.first /= d; p.second /= d;
}
```

# 2 đoạn thẳng cắt nhau

```cpp
point a, b, c, d;
	if (ccw(a, c, d) * ccw(b, c, d) < 0 && ccw(c, a, b) * ccw(d, a, b) < 0) return true;
	if (InLine(a, c, d) || InLine(b, c, d) || InLine(c, a, b) || InLine(d, a, b)) return true;
return false;
```

# Bao lồi

```cpp
vector<point> a(N), b(N);
    for (int i = 0; i < n; ++i) cin >> a[i].x >> a[i].y;
    sort(a.begin(), a.begin() + n);
    int m = 0;
    for (int i = 0; i < n; ++i) {
        while (m >= 2 && cross(b[m - 2], b[m - 1], a[i]) <= 0) --m;
        b[m++] = a[i];
    }
    int m1 = m;
    for (int i = n - 2; i >= 0; --i) {
        while (m > m1 && cross(b[m - 2], b[m - 1], a[i]) <= 0) --m;
        b[m++] = a[i];
    }
    double per = 0, area = 0;
    for (int i = 0; i < m - 1; ++i) {
        per += sqrt(dispoint(b[i], b[i + 1]));
        area += b[i] * b[i + 1];
    }
    area /= 2;
```

# Kiểm tra điểm trong bao lồi (không tính biên)

```cpp
bool in_convex(point M) {
    int lo = 2, hi = n;
    if (cross(a[lo], a[hi], a[1]) > 0) swap(lo, hi);
    if (cross(a[1], a[lo], M) >= 0 || cross(a[1], a[hi], M) <= 0) return false;
    while (abs(hi - lo) > 1) {
        int mid = (lo + hi) / 2;
        if (cross(a[1], a[mid], M) > 0)
            hi = mid;
        else
            lo = mid;
    }
    return (cross(a[lo], a[hi], M) < 0);
```

# 2 con trỏ

- 2 điểm

```cpp
int ans = 0;
	int j = 1;
	for (int i = 1; i <= n; ++i) {
		while (j < n && dispoint(a[i], a[j]) <  dispoint(a[i], a[j + 1])) ++j;
		ans = max(ans, dispoint(a[i], a[j]));
	}
```

- Tam giác

```cpp
int ans = 0;
	for (int i = 0; i < m; ++i) {
		int k = i + 1;
		for (int j = i + 1; j < m - 1; ++j) {
			while (k < j && area(b[i], b[j], b[k]) < area(b[i], b[j], b[k + 1])) ++k;
			ans = max(ans, area(b[i], b[j], b[k]));
		}	
	}
```

- Tứ giác

```cpp
#include <bits/stdc++.h>
using namespace std;

#define f first
#define s second
typedef pair<int, int> point;
double operator * (point A, point B) {
	return A.f * B.s - B.f * A.s;
}

const int N = 4e3 + 5;
point p[N];
int n;

double area(int a, int b, int c) {
    point x[5];
    x[1] = p[a]; x[2] = p[b]; x[3] = p[c]; x[4] = p[a];
    double ans = 0;
    for (int i = 1; i <= 3; ++i) ans += x[i] * x[i + 1];
    return abs(ans / 2);
}

double find(int u, int v, int x, int y) {
	int lo = u, hi = v;
	while (hi - lo > 9) {
		int pointA = lo + 4 * (hi - lo) / 9;
		int pointB = hi - 4 * (hi - lo) / 9;
		if (area(x, pointA, y) < area(x, pointB, y))
			lo = pointA;
		else
			hi = pointB;
	}
	double ans = area(x, lo, y);
	for (int i = lo; i <= hi; ++i) 
		ans = max(ans, area(x, i, y));
	return ans;
} 

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);

    cin >> n;
    for (int i = 1; i <= n; ++i) cin >> p[i].f >> p[i].s;
    for (int i = 1; i <= n; ++i) p[i + n] = p[i];

    double ans = 0;
    for (int i = 1; i <= n; ++i) {
    	for (int j = i + 2; j <= n; ++j) {
    		double A1 = find(i + 1, j - 1, i, j);
    		double A2 = find(j + 1, i + n, i, j);
    		ans = max(ans, A1 + A2);
    	}
    }

    cout << fixed << setprecision(1) << ans;

    return 0;
}
```

# Giao 2 đa giác

```cpp
void FindIntersect(point u, point v) {
    int sign1 = ccw(u, v, a[1]);
    if (sign1 >= 0) c[++r] = a[1];
    for (int i = 2; i <= n + 1; ++i) {
        int sign2 = ccw(u, v, a[i]);
        if (sign1 * sign2 < 0) {
            point d = intersect(a[i - 1], a[i], u, v);
            c[++r] = d;
        }
        if (sign2 >= 0) c[++r] = a[i];
        sign1 = sign2;
    }
}
bool solve() {
    cin >> n;
    if (n == 0) return 0;
    for (int i = 1; i <= n; ++i) cin >> a[i].x >> a[i].y;
    cin >> m;
    for (int i = 1; i <= m; ++i) cin >> b[i].x >> b[i].y;
    if (area(a, n) < 0) reverse(a + 1, a + n + 1);
    if (area(b, m) < 0) reverse(b + 1, b + m + 1);
    double areaA = abs(area(a, n));
    double areaB = abs(area(b, m));
    b[m + 1] = b[1];
    for (int i = 1; i <= m; ++i) {
        r = 0;
        a[n + 1] = a[1];
        FindIntersect(b[i], b[i + 1]);
        for (int j = 1; j <= r; ++j) a[j] = c[j];
        n = r;
    }
    double areaC = abs(area(a, n)) * 2;
    cout << fixed << setprecision(2) << areaA + areaB - areaC << '\n';
//    cout << fixed << setprecision(2) << areaA << ' ' << areaB << ' ' << areaC << '\n';
    cout.precision(2);
    cout << fixed;
//    r = 0;
//    cout << n << '\n';
//    FindIntersect(b[1], b[2]);
//    for (int i = 1; i <= r; ++i) cout << c[i].x << ' ' << c[i].y << '\n';

    return 1;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    while (solve());
}
```