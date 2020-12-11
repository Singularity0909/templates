> **GitHub [README.md](https://github.com/Singularity0909/templates/blob/main/README.md)** 无法加载 **MathJax** 数学公式，可以通过安装[浏览器插件](https://chrome.google.com/webstore/detail/mathjax-plugin-for-github/ioemnmodlmafdkllaclgeombjnmnbima/related)解决。

## IO 及其他

#### 快速读写

```cpp
template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' or ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' and ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}
```

#### 关闭同步流

```cpp
ios::sync_with_stdio(false);
```

#### 文件重定向

```cpp
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
    freopen("output.txt", "w", stdout);
#endif
```

#### 随机数

```cpp
default_random_engine generator { random_device {} () };
shuffle(nums.begin(), nums.end(), generator);
```

#### 运行时间

```cpp
clock_t start, end;
start = clock();
// TODO
end = clock();
cout << double(end - start) / CLOCKS_PER_SEC << "s" << endl;
```

#### Lambda

```cpp
auto cmp = [](int a, int b) -> bool { return a > b; };
priority_queue<int, vector<int>, decltype(cmp)> q(cmp);
```

#### 万能头文件

```cpp
// C++ includes used for precompiling -*- C++ -*-
 
// Copyright (C) 2003-2014 Free Software Foundation, Inc.
//
// This file is part of the GNU ISO C++ Library.  This library is free
// software; you can redistribute it and/or modify it under the
// terms of the GNU General Public License as published by the
// Free Software Foundation; either version 3, or (at your option)
// any later version.
 
// This library is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.
 
// Under Section 7 of GPL version 3, you are granted additional
// permissions described in the GCC Runtime Library Exception, version
// 3.1, as published by the Free Software Foundation.
 
// You should have received a copy of the GNU General Public License and
// a copy of the GCC Runtime Library Exception along with this program;
// see the files COPYING3 and COPYING.RUNTIME respectively.  If not, see
// <http://www.gnu.org/licenses/>.
 
/** @file stdc++.h
 *  This is an implementation file for a precompiled header.
 */
 
// 17.4.1.2 Headers
 
// C
#ifndef _GLIBCXX_NO_ASSERT
#include <cassert>
#endif
#include <cctype>
#include <cerrno>
#include <cfloat>
#include <ciso646>
#include <climits>
#include <clocale>
#include <cmath>
#include <csetjmp>
#include <csignal>
#include <cstdarg>
#include <cstddef>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <ctime>
 
#if __cplusplus >= 201103L
#include <ccomplex>
#include <cfenv>
#include <cinttypes>
#include <cstdbool>
#include <cstdint>
#include <ctgmath>
#include <cwchar>
#include <cwctype>
#endif
 
// C++
#include <algorithm>
#include <bitset>
#include <complex>
#include <deque>
#include <exception>
#include <fstream>
#include <functional>
#include <iomanip>
#include <ios>
#include <iosfwd>
#include <iostream>
#include <istream>
#include <iterator>
#include <limits>
#include <list>
#include <locale>
#include <map>
#include <memory>
#include <new>
#include <numeric>
#include <ostream>
#include <queue>
#include <set>
#include <sstream>
#include <stack>
#include <stdexcept>
#include <streambuf>
#include <string>
#include <typeinfo>
#include <utility>
#include <valarray>
#include <vector>
 
#if __cplusplus >= 201103L
#include <array>
#include <atomic>
#include <chrono>
#include <condition_variable>
#include <forward_list>
#include <future>
#include <initializer_list>
#include <mutex>
#include <random>
#include <ratio>
#include <regex>
#include <scoped_allocator>
#include <system_error>
#include <thread>
#include <tuple>
#include <typeindex>
#include <type_traits>
#include <unordered_map>
#include <unordered_set>
#endif
```

## 字符串

#### 字符串分割

```cpp
vector<string> split(const string& s, const string& sep)
{
    vector<string> res;
    string::size_type pos1, pos2;
    pos2 = s.find(sep);
    pos1 = 0;
    while (string::npos != pos2) {
        res.push_back(s.substr(pos1, pos2 - pos1));
        pos1 = pos2 + sep.size();
        pos2 = s.find(sep, pos1);
    }
    if (pos1 != s.length()) {
        res.push_back(s.substr(pos1));
    }
    return res;
}
```

#### KMP 算法

```cpp
void getNext(char* p)
{
    int len = (int)strlen(p + 1);
    for (int i = 2, j = 0; i <= len; i++) {
        while (j && p[i] != p[j + 1]) j = nxt[j];
        if (p[i] == p[j + 1]) j++;
        nxt[i] = j;
    }
}

void kmp(char* s, char* p)
{
    int slen = (int)strlen(s + 1), plen = (int)strlen(p + 1);
    for (int i = 1, j = 0; i <= slen; i++) {
        while (j && s[i] != p[j + 1]) j = nxt[j];
        if (s[i] == p[j + 1]) j++;
        if (j == plen) {
            printf("%d\n", i - plen + 1);
            j = nxt[j];
        }
    }
}
```

#### Manacher 算法

[LeetCode 5 Longest Palindromic Substring](https://leetcode-cn.com/problems/longest-palindromic-substring/)

```cpp
class Solution {
public:
    string longestPalindrome(string s)
    {
        int len = (int)s.length();
        int n = len * 2 + 2;
        vector<int> mp(n, 0);
        string ma("$#");
        for (int i = 0; i < len; ++i) {
            ma.push_back(s[i]);
            ma.push_back('#');
        }
        int mx = 0, id = 0, p = 0;
        for (int i = 0; i < n; ++i) {
            mp[i] = mx > i ? min(mx - i, mp[id * 2 - i]) : 1;
            if (i == 0) continue;
            while (ma[i + mp[i]] == ma[i - mp[i]]) {
                ++mp[i];
            }
            if (mx < mp[i] + i) {
                mx = mp[i] + i;
                id = i;
            }
            if (mp[i] > mp[p]) {
                p = i;
            }
        }
        len = mp[p] - 1;
        p = (p - len) / 2;
        return s.substr(p, len);
    }
};
```

[洛谷 P3805 【模板】manacher算法](https://www.luogu.com.cn/problem/P3805)

```cpp
#include <bits/stdc++.h>

using namespace std;
const int maxn = 11e6 + 10;
int mp[maxn << 1];
char s[maxn], ma[maxn << 1];

void manacher(int len)
{
    memset(mp, 0, sizeof mp);
    int l = 0;
    ma[l++] = '$';
    ma[l++] = '#';
    for (int i = 0; i < len; ++i) {
        ma[l++] = s[i];
        ma[l++] = '#';
    }
    ma[l] = '\0';
    int mx = 0, id = 0;
    for (int i = 0; i < l; ++i) {
        mp[i] = mx > i ? min(mx - i, mp[id * 2 - i]) : 1;
        while (ma[i + mp[i]] == ma[i - mp[i]]) ++mp[i];
        if (mx < mp[i] + i) {
            mx = mp[i] + i;
            id = i;
        }
    }
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    scanf("%s", s);
    int len = (int)strlen(s);
    manacher(len);
    int res = 0;
    for (int i = 0; i < len * 2 + 2; ++i) {
        res = max(res, mp[i] - 1);
    }
    printf("%d\n", res);
    return 0;
}
```

#### AC 自动机

[洛谷 P3808 【模板】AC自动机（简单版）](https://www.luogu.com.cn/problem/P3808)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(1e6 + 10);
char s[maxn];
int idx, trie[maxn][26];
int ext[maxn], fail[maxn];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' or ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' and ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

void insert(char* str)
{
    int u = 0;
    for (int i = 0; str[i]; ++i) {
        int c = str[i] - 'a';
        if (not trie[u][c]) {
            trie[u][c] = ++idx;
        }
        u = trie[u][c];
    }
    ++ext[u];
}

void build()
{
    queue<int> q;
    for (int i = 0; i < 26; ++i) {
        if (trie[0][i]) {
            q.push(trie[0][i]);
        }
    }
    while (not q.empty()) {
        int u = q.front();
        q.pop();
        for (int i = 0; i < 26; ++i) {
            int& v = trie[u][i];
            if (v) {
                fail[v] = trie[fail[u]][i];
                q.push(v);
            } else {
                v = trie[fail[u]][i];
            }
        }
    }
}

int query(char* str)
{
    int u = 0, res = 0;
    for (int i = 0; str[i]; ++i) {
        int c = str[i] - 'a';
        u = trie[u][c];
        for (int j = u; j and compl ext[j]; j = fail[j]) {
            res += ext[j];
            ext[j] = -1;
        }
    }
    return res;
}

int main()
{
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
#endif
    int n = read();
    for (int i = 0; i < n; ++i) {
        scanf("%s", s);
        insert(s);
    }
    build();
    scanf("%s", s);
    write(query(s), true);
    return 0;
}
```

[洛谷 P3796 【模板】AC自动机（加强版）](https://www.luogu.com.cn/problem/P3796)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(160);
const int maxl(1e6 + 10);
const int maxs(maxn * 80);
char s[maxn][maxn], t[maxl];
int tot, trie[maxs][26];
int idx[maxs], fail[maxs], cnt[maxn];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' or ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' and ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

void init()
{
    tot = 0;
    memset(trie, 0, sizeof(trie));
    memset(fail, 0, sizeof(fail));
    memset(idx, 0, sizeof idx);
    memset(cnt, 0, sizeof cnt);
}

void insert(char* str, int id)
{
    int u = 0;
    for (int i = 1; str[i]; ++i) {
        int c = str[i] - 'a';
        if (not trie[u][c]) {
            trie[u][c] = ++tot;
        }
        u = trie[u][c];
    }
    idx[u] = id;
}

void build()
{
    queue<int> q;
    for (int i = 0; i < 26; ++i) {
        if (trie[0][i]) {
            q.push(trie[0][i]);
        }
    }
    while (not q.empty()) {
        int u = q.front();
        q.pop();
        for (int i = 0; i < 26; ++i) {
            int& v = trie[u][i];
            if (v) {
                fail[v] = trie[fail[u]][i];
                q.push(v);
            } else {
                v = trie[fail[u]][i];
            }
        }
    }
}

int query(char* str, int n)
{
    int u = 0, res = 0;
    for (int i = 1; str[i]; ++i) {
        int c = str[i] - 'a';
        u = trie[u][c];
        for (int j = u; j; j = fail[j]) {
            ++cnt[idx[j]];
        }
    }
    for (int i = 1; i <= n; ++i) {
        res = max(res, cnt[i]);
    }
    return res;
}

int main()
{
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
#endif
    int n;
    while (compl scanf("%d", bitand n) and n) {
        init();
        for (int i = 1; i <= n; ++i) {
            scanf("%s", s[i] + 1);
            insert(s[i], i);
        }
        build();
        scanf("%s", t + 1);
        int res = query(t, n);
        write(res, true);
        for (int i = 1; i <= n; ++i) {
            if (cnt[i] == res) {
                puts(s[i] + 1);
            }
        }
    }
    return 0;
}
```

#### 高精度

```cpp
#define MAXLEN 10010
#define MAXN 9999
#define MAXSIZE 1010
#define DLEN 4
 
class BigNum
{
private:
    int a[MAXLEN];
    int len;
public:
    BigNum() { len = 1; memset(a, 0, sizeof(a)); }
    BigNum(const int);
    BigNum(const char*);
    BigNum(const BigNum&);
    BigNum& operator =(const BigNum &);
    friend istream& operator >>(istream&, BigNum&);
    friend ostream& operator <<(ostream&, BigNum&);
    BigNum operator +(const BigNum&) const;
    BigNum operator -(const BigNum&) const;
    BigNum operator *(const BigNum&) const;
    BigNum operator /(const int&) const;
    BigNum operator ^(const int&) const;
    int operator %(const int&) const;
    bool operator >(const BigNum& T)const;
    bool operator >(const int& t)const;
    void print();
};
 
BigNum::BigNum(const int b)
{
    int c, d = b;
    len = 0;
    memset(a, 0, sizeof(a));
    while (d > MAXN)
    {
        c = d - (d / (MAXN + 1)) * (MAXN + 1);
        d = d / (MAXN + 1);
        a[len++] = c;
    }
    a[len++] = d;
}
 
BigNum::BigNum(const char* s)
{
    int t, k, index, L, i;
    memset(a, 0, sizeof(a));
    L = (int)strlen(s);
    len = L / DLEN;
    if (L % DLEN) len++;
    index = 0;
    for (i = L - 1; i >= 0; i -= DLEN)
    {
        t = 0;
        k = i - DLEN + 1;
        if (k < 0) k = 0;
        for (int j = k; j <= i; j++)
            t = t * 10 + s[j] - '0';
        a[index++] = t;
    }
}
 
BigNum::BigNum(const BigNum& T) : len(T.len)
{
    int i;
    memset(a, 0, sizeof(a));
    for (i = 0; i < len; i++) a[i] = T.a[i];
}
 
BigNum& BigNum::operator =(const BigNum& n)
{
    int i;
    len = n.len;
    memset(a, 0, sizeof(a));
    for (i = 0; i < len; i++) a[i] = n.a[i];
    return *this;
}
 
istream& operator >>(istream& in, BigNum& b)
{
    char ch[MAXSIZE * 4];
    int i = -1;
    in >> ch;
    int L = (int)strlen(ch);
    int count = 0, sum = 0;
    for (i = L - 1; i >= 0;)
    {
        sum = 0;
        int t = 1;
        for (int j = 0; j < 4 && i >= 0; j++, i--, t *= 10)
            sum += (ch[i] - '0') * t;
        b.a[count] = sum;
        count++;
    }
    b.len = count++;
    return in;
}
 
ostream& operator <<(ostream& out, BigNum& b)
{
    int i;
    cout << b.a[b.len - 1];
    for (i = b.len - 2; i >= 0; i--)
        printf("%04d", b.a[i]);
    return out;
}
    
BigNum BigNum::operator +(const BigNum& T) const
{
    BigNum t(*this);
    int i, big;
    big = T.len > len ? T.len : len;
    for (i = 0; i < big; i++)
    {
        t.a[i] += T.a[i];
        if (t.a[i] > MAXN)
        {
            t.a[i + 1]++;
            t.a[i] -= MAXN + 1;
        }
    }
    if (t.a[big] != 0) t.len = big + 1;
    else t.len = big;
    return t;
}
 
BigNum BigNum::operator -(const BigNum& T) const
{
    int i, j, big;
    bool flag;
    BigNum t1, t2;
    if (*this > T)
    {
        t1 = *this;
        t2 = T;
        flag = 0;
    }
    else
    {
        t1 = T;
        t2 = *this;
        flag = 1;
    }
    big = t1.len;
    for (i = 0; i < big; i++)
    {
        if (t1.a[i] < t2.a[i])
        {
            j = i + 1;
            while (t1.a[j] == 0) j++;
            t1.a[j--]--;
            while (j > i) t1.a[j--] += MAXN;
            t1.a[i] += MAXN + 1 - t2.a[i];
        }
        else t1.a[i] -= t2.a[i];
    }
    t1.len = big;
    while (t1.a[t1.len - 1] == 0 && t1.len > 1)
    {
        t1.len--;
        big--;
    }
    if (flag) t1.a[big - 1] = 0 - t1.a[big - 1];
    return t1;
}
 
BigNum BigNum::operator *(const BigNum& T)const
{
    BigNum ret;
    int i, j = 0, up;
    int temp, temp1;
    for (i = 0; i < len; i++)
    {
        up = 0;
        for (j = 0; j < T.len; j++)
        {
            temp = a[i] * T.a[j] + ret.a[i + j] + up;
            if (temp > MAXN)
            {
                temp1 = temp - temp / (MAXN + 1) * (MAXN + 1);
                up = temp / (MAXN + 1);
                ret.a[i + j] = temp1;
            }
            else
            {
                up = 0;
                ret.a[i + j] = temp;
            }
        }
        if (up != 0) ret.a[i + j] = up;
    }
    ret.len = i + j;
    while (ret.a[ret.len - 1] == 0 && ret.len > 1) ret.len--;
    return ret;
}
 
BigNum BigNum::operator /(const int& b)const
{
    BigNum ret;
    int i, down = 0;
    for (i = len - 1; i >= 0; i--)
    {
        ret.a[i] = (a[i] + down * (MAXN + 1)) / b;
        down = a[i] + down * (MAXN + 1) - ret.a[i] * b;
    }
    ret.len = len;
    while (ret.a[ret.len - 1] == 0 && ret.len > 1) ret.len--;
    return ret;
}
 
int BigNum::operator %(const int& b) const
{
    int i, d = 0;
    for (i = len - 1; i >= 0; i--)
        d = ((d * (MAXN + 1)) % b + a[i]) % b;
    return d;
}
 
BigNum BigNum::operator ^(const int& n) const
{
    BigNum t, ret(1);
    int i;
    if (n < 0) exit(-1);
    if (n == 0) return 1;
    if (n == 1) return *this;
    int m = n;
    while (m > 1)
    {
        t = *this;
        for (i = 1; (i << 1) <= m; i <<= 1) t = t * t;
        m -= i;
        ret = ret * t;
        if (m == 1) ret = ret * (*this);
    }
    return ret;
}
 
bool BigNum::operator >(const BigNum& T) const
{
    int ln;
    if (len > T.len) return true;
    else if (len == T.len)
    {
        ln = len - 1;
        while (a[ln] == T.a[ln] && ln >= 0) ln--;
        if (ln >= 0 && a[ln] > T.a[ln]) return true;
        else return false;
    }
    else return false;
}
 
bool BigNum::operator >(const int& t) const
{
    BigNum b(t);
    return *this > b;
}
 
void BigNum::print()
{
    int i;
    printf("%d", a[len - 1]);
    for (i = len - 2; i >= 0; i--)
        printf("%04d", a[i]);
    printf("\n");
}
```

## 数学

#### 快速幂

```cpp
ll qpow(ll x, ll k)
{
    ll res = 1;
    while (k) {
        if (k & 1) {
            res = res * x % mod;
        }
        x = x * x % mod;
        k >>= 1;
    }
    return res;
}
```

#### 矩阵快速幂

[洛谷 P3390 【模板】矩阵快速幂](https://www.luogu.com.cn/problem/P3390)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int mod(1e9 + 7);
const int maxn(1e2 + 10);

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' or ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' and ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, char c)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (c) putchar(c);
}

struct matrix {
    int n;
    ll val[maxn][maxn];
    
    matrix(vector<vector<int>> vec)
    {
        assert(vec.size() == vec.front().size());
        this->n = (int)vec.size();
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                val[i][j] = vec[i][j];
            }
        }
    }
    
    matrix(int n, bool id = false)
    {
        this->n = n;
        memset(val, 0, sizeof val);
        if (not id) return;
        // identity matrix
        for (int i = 0; i < n; ++i) {
            val[i][i] = id;
        }
    }
    
    matrix operator *(const matrix& rhs) {
        assert(n == rhs.n);
        matrix res(n);
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < n; ++k) {
                    res.val[i][j] = (res.val[i][j] + val[i][k] * rhs.val[k][j] % mod) % mod;
                }
            }
        }
        return res;
    };
    
    matrix operator ^(ll k)
    {
        matrix res(n, true), x = *this;
        while (k) {
            if (k bitand 1) {
                res = res * x;
            }
            x = x * x;
            k >>= 1;
        }
        return res;
    }
};

int main()
{
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
#endif
    int n = read();
    ll k = read<ll>();
    matrix mat(n);
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            mat.val[i][j] = read();
        }
    }
    mat = mat ^ k;
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            write(mat.val[i][j], j == n - 1 ? '\n' : ' ');
        }
    }
    return 0;
}
```

#### 筛法

###### 欧拉筛法

```cpp
void euler(int n, vector<int>& primes)
{
    primes.clear();
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i <= n; i++) {
        if (isPrime[i]) {
            primes.push_back(i);
        }
        for (int p : primes) {
            if (1ll * i * p > n) break;
            isPrime[i * p] = false;
            if (i % p == 0) break;
        }
    }
}
```

埃拉托斯特尼筛法

```cpp
void eratosthenes(int n, vector<int>& primes)
{
    primes.clear();
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i <= n; i++) {
        if (!isPrime[i]) continue;
        primes.push_back(i);
        if (ll(i) * i > n) continue;
        for (int j = i * i; j <= n; j += i) {
            isPrime[j] = false;
        }
    }
}
```

#### 欧拉函数

###### 欧拉函数

欧拉函数（Euler's totient function），即 $\varphi(n)$ ，表示的是小于等于 $n$ 和 $n$ 互质的数的个数。

比如说 $\varphi(1) = 1$ 。

当 $n$ 是质数的时候，显然有 $\varphi(n) = n - 1$ 。

利用唯一分解定理，我们可以把一个整数唯一地分解为质数幂次的乘积。

设 $n = \prod_{i=1}^{n}p_i^{k_i}$，其中 $p_i$ 是质数，那么 $\varphi(n) = n \times \prod_{i = 1}^s{\dfrac{p_i - 1}{p_i}}$ 。

###### 欧拉定理

若 $\gcd(a, m) = 1$ ，则$a^{\varphi(m)} \equiv 1 \pmod{m}$ 。

###### 费马小定理

若 $p$ 为素数， $\gcd(a, p) = 1$ ，则 $a^{p - 1} \equiv 1 \pmod{p}$ 。

###### 威尔逊定理

$(p-1)!\equiv-1\pmod{p}$

若 $p$ 为质数，则 $p$ 可整除 $(p-1)!+1$ 。

###### 筛法求欧拉函数

```cpp
void phi_table()
{
    for (int i = 1; i < maxn; i++) {
        phi[i] = i;
    }
    for (int i = 2; i < maxn; i++) {
        if (phi[i] == i) {
            for (int j = i; j < maxn; j += i) {
                phi[j] = phi[j] / i * (i - 1);
            }
        }
    }
}
```

###### Pollard Rho 算法求欧拉函数

```cpp
ll get_phi(int n)
{
    ll res = 1;
    for (int i = 2; i <= sqrt(n); i++) {
        if (n % i == 0) {
            n /= i;
            res *= i - 1;
            while (n % i == 0) {
                n /= i;
                res *= i;
            }
        }
    }
    if (n > 1) {
        res *= n - 1;
    }
    return res;
}
```

#### Polya 定理

$m$ 颗珠子，$n$ 种颜色： $L=\dfrac{1}{m}\cdot \sum ^{m}_{i=1}n^{\gcd \left( i,m\right) }$

当 $n=m$ ：$L=\dfrac{1}{n}\sum _{p|n}(\varphi\left(p\right)\cdot n^{\dfrac{n}{p}})=\sum _{p|n}(\varphi\left(p\right)\cdot n^{\dfrac{n}{p}-1}）$

#### 逆元

###### 扩展欧几里得法求逆元

```cpp
void exgcd(int a, int b, int& x, int& y)
{
    if (b == 0) {
        x = 1;
        y = 0;
        return;
    }
    exgcd(b, a % b, y, x);
    y -= a / b * x;
}
```

###### 快速幂法求逆元

因为 $ax \equiv 1 \pmod b$ ；

所以 $ax \equiv a^{b-1} \pmod b$（根据 [费马小定理](https://./fermat.md) ）；

所以 $x \equiv a^{b-2} \pmod b$。

###### 线性求逆元

求出 $1,2,...,n$ 中每个数关于 $p$ 的逆元。

首先，很显然的 $1^{-1} \equiv 1 \pmod p$ ；

然后，设 $p=ki+j,j<i,1<i<p$ ，再放到 $\mod p$ 意义下就会得到：$ki+j \equiv 0 \pmod p$；

两边同时乘 $i^{-1},j^{-1}$ ：

$kj^{-1}+i^{-1} \equiv 0 \pmod p$；

$i^{-1} \equiv -kj^{-1} \pmod p$；

$i^{-1} \equiv -(\frac{p}{i}) (p \bmod i)^{-1}$；

```cpp
inv[i] = 1;
for (int i = 2; i <= n; i++) {
    inv[i] = (ll)(p - p / i) * inv[p % i] % p;
}
```

#### 组合数

###### Pascal 公式打表组合数取模

```cpp
const int maxn = 1e3 + 5;
const int mod = 1e5 + 3;
int tC[maxn][maxn];

inline int C(int n, int k)
{
    if (k > n) return 0;
    return tC[n][k];
}

void calcC(int n)
{
    for (int i = 0; i < n; i++) {
        tC[i][0] = 1;
        for (int j = 1; j < i; j++) {
            tC[i][j] = (C(i - 1, j - 1) + C(i - 1, j)) % mod;
        }
        tC[i][i] = 1;
    }
}
```

```cpp
const int maxn = 1e3 + 5;
const int mod = 1e5 + 3;
int tC[maxn * maxn];

inline int loc(int n, int k)
{
    int pos = (1 + (n >> 1)) * (n >> 1);
    pos += k;
    pos += (n & 1) ? (n + 1) >> 1 : 0;
    return pos;
}

inline int C(int n, int k)
{
    if (k > n) return 0;
    k = min(n - k, k);
    return tC[loc(n, k)];
}

void calcC(int n)
{
    for (int i = 0; i < n; i++) {
        tC[loc(i, 0)] = 1;
        for (int j = 1, e = i >> 1; j <= e; j++) {
            tC[loc(i, j)] = C(i - 1, j) + C(i - 1, j - 1);
        }
    }
}
```

## 图论

#### Dijkstra 算法

[洛谷 P4779 【模板】单源最短路径（标准版）](https://www.luogu.com.cn/problem/P4779)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const ll _inf(0x3f3f3f3f3f3f3f3f);
const int mod(1e9 + 7);
const int maxn(1e5 + 10);
const int maxm(2e5 + 10);
int ecnt, head[maxn];
ll dis[maxn];
bool vis[maxn];

struct edge {
    int to, wt, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, char c)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (c) putchar(c);
}

void addEdge(int u, int v, int w)
{
    edges[ecnt].to = v;
    edges[ecnt].wt = w;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

void dijkstra(int src)
{
    priority_queue<p, vector<p>, greater<p>> q;
    q.push(p(0, src));
    dis[src] = 0;
    while (not q.empty()) {
        int u = q.top().second;
        q.pop();
        if (vis[u]) continue;
        vis[u] = true;
        for (int i = head[u]; ~i; i = edges[i].nxt) {
            int v = edges[i].to, w = edges[i].wt;
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                q.push(p(dis[v], v));
            }
        }
    }
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    memset(head, -1, sizeof head);
    memset(dis, 0x3f, sizeof dis);
    int n = read(), m = read(), s = read();
    while (m--) {
        int u = read(), v = read(), w = read();
        addEdge(u, v, w);
    }
    dijkstra(s);
    for (int i = 1; i <= n; ++i) {
        write(dis[i] == _inf ? INT_MAX : dis[i], i == n ? '\n' : ' ');
    }
    return 0;
}
```

#### SPFA 算法

[洛谷 P3371 【模板】单源最短路径（弱化版）](https://www.luogu.com.cn/problem/P3371)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const ll _inf(0x3f3f3f3f3f3f3f3f);
const int mod(1e9 + 7);
const int maxn(1e4 + 10);
const int maxm(5e5 + 10);
int ecnt, head[maxn];
ll dis[maxn];
bool vis[maxn];

struct edge {
    int to, wt, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, char c)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (c) putchar(c);
}

void addEdge(int u, int v, int w)
{
    edges[ecnt].to = v;
    edges[ecnt].wt = w;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

void spfa(int src)
{
    queue<int> q;
    q.push(src);
    dis[src] = 0;
    vis[src] = true;
    while (not q.empty()) {
        int u = q.front();
        q.pop();
        vis[u] = false;
        for (int i = head[u]; ~i; i = edges[i].nxt) {
            int v = edges[i].to, w = edges[i].wt;
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                if (not vis[v]) {
                    q.push(v);
                    vis[v] = true;
                }
            }
        }
    }
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    memset(head, -1, sizeof head);
    memset(dis, 0x3f, sizeof dis);
    int n = read(), m = read(), s = read();
    while (m--) {
        int u = read(), v = read(), w = read();
        addEdge(u, v, w);
    }
    spfa(s);
    for (int i = 1; i <= n; ++i) {
        write(dis[i] == _inf ? INT_MAX : dis[i], i == n ? '\n' : ' ');
    }
    return 0;
}
```

玄学优化 https://www.luogu.com.cn/blog/xhhkwy/spfa-hacker-orzorz

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const int mod(1e9 + 7);
const int maxn(1e5 + 10);
const int maxm(1e6 + 10);
int ecnt, head[maxn];
int dis[maxn];
bool vis[maxn];
deque<int> q;

struct edge {
    int to, wt, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, char c)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (c) putchar(c);
}

void addEdge(int u, int v, int w)
{
    edges[ecnt].to = v;
    edges[ecnt].wt = w;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

void swap_slf()
{
    if (q.size() > 2 and dis[q.front()] > dis[q.back()]) {
        swap(q.front(), q.back());
    }
}

void spfa(int src)
{
    dis[src] = 0;
    q.push_back(src);
    vis[src] = true;
    while (not q.empty()) {
        int u = q.front();
        q.pop_front();
        swap_slf();
        vis[u] = false;
        for (int i = head[u]; compl i; i = edges[i].nxt) {
            int v = edges[i].to, w = edges[i].wt;
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                if (not vis[v]) {
                    q.push_back(v);
                    swap_slf();
                    vis[v] = true;
                }
            }
        }
    }
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    memset(head, -1, sizeof head);
    memset(dis, 0x3f, sizeof dis);
    int n = read(), m = read(), s = read();
    while (m--) {
        int u = read(), v = read(), w = read();
        addEdge(u, v, w);
    }
    spfa(s);
    for (int i = 1; i <= n; ++i) {
        if (dis[i] == inf) {
            puts("NO PATH");
        } else {
            write(dis[i], i == n ? 10 : 32);
        }
    }
    return 0;
}
```

[洛谷 P3385 【模板】负环](https://www.luogu.com.cn/problem/P3385)

判断存在负环的条件：1、当前点入列达到 $n$ 次；2、源点到当前点的最短（长）路径达到 $n$。

两种玄学优化：1、入列计数 `tot >= n * 2` 跳出；2、使用 `stack` 代替 `queue`。

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const ll _inf(0x3f3f3f3f3f3f3f3f);
const int mod(1e9 + 7);
const int maxn(2e3 + 10);
const int maxm(3e3 + 10);
bool vis[maxn];
int ecnt, head[maxn], cnt[maxn], dis[maxn];

struct edge {
    int to, wt, nxt;
} edges[maxm << 1];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, char c)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (c) putchar(c);
}

void addEdge(int u, int v, int w)
{
    edges[ecnt].to = v;
    edges[ecnt].wt = w;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

bool spfa(int tot)
{
    queue<int> q;
    vis[1] = true;
    dis[1] = 0;
    q.push(1);
    while (not q.empty()) {
        int u = q.front();
        q.pop();
        vis[u] = false;
        for (int i = head[u]; ~i; i = edges[i].nxt) {
            int v = edges[i].to, w = edges[i].wt;
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                if (not vis[v]) {
                    cnt[v] = cnt[u] + 1;
                    if (cnt[v] >= tot) {
                        return true;
                    }
                    vis[v] = true;
                    q.push(v);
                }
            }
        }
    }
    return false;
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    int t = read();
    while (t--) {
        ecnt = 0;
        memset(head, -1, sizeof head);
        memset(dis, 0x3f, sizeof dis);
        memset(vis, false, sizeof vis);
        memset(cnt, 0, sizeof cnt);
        int n = read(), m = read();
        while (m--) {
            int u = read(), v = read(), w = read();
            addEdge(u, v, w);
            if (w >= 0) {
                addEdge(v, u, w);
            }
        }
        printf("%s\n", spfa(n) ? "YES" : "NO");
    }
    return 0;
}
```

[洛谷 P5960 【模板】差分约束算法](https://www.luogu.com.cn/problem/P5960)

差分约束：求最小值→最长路，求最大值→最短路

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const int mod(1e9 + 7);
const int maxn(5e3 + 10);
const int maxm(1e4 + 10);
bool vis[maxn];
int ecnt, head[maxn], cnt[maxn], dis[maxn];

struct edge {
    int to, wt, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, char c)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (c) putchar(c);
}

void addEdge(int u, int v, int w)
{
    edges[ecnt].to = v;
    edges[ecnt].wt = w;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

bool spfa(int n)
{
    memset(dis, 0x3f, sizeof dis);
    dis[0] = 0;
    queue<int> q;
    q.push(0);
    vis[0] = true;
    while (not q.empty()) {
        int u = q.front();
        q.pop();
        vis[u] = false;
        for (int i = head[u]; compl i; i = edges[i].nxt) {
            int v = edges[i].to, w = edges[i].wt;
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                if (not vis[v]) {
                    vis[v] = true;
                    q.push(v);
                    cnt[v] = cnt[u] + 1;
                    if (cnt[v] >= n + 1) {
                        return false;
                    }
                }
            }
        }
    }
    return true;
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    ios::sync_with_stdio(false);
    memset(head, -1, sizeof head);
    int n = read(), m = read();
    while (m--) {
        int u = read(), v = read(), w = read();
        addEdge(v, u, w);
    }
    for (int i = 0; i <= n; ++i) {
        addEdge(0, i, 0);
    }
    if (spfa(n)) {
        for (int i = 1; i <= n; ++i) {
            write(dis[i], " \n"[i == n]);
        }
    } else {
        printf("NO\n");
    }
    return 0;
}
```

#### Kruskal 算法

[洛谷 P3366 【模板】最小生成树](https://www.luogu.com.cn/problem/P3366)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(5e3 + 10);
const int maxm(2e5 + 10);
int pre[maxn];

struct edge {
    int u, v, w;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' or ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' and ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline int Find(int cur)
{
    return pre[cur] == cur ? cur : pre[cur] = Find(pre[cur]);
}

inline void Union(int u, int v)
{
    pre[Find(v)] = Find(u);
}

int kruskal(int n, int m)
{
    int sum = 0, cnt = 0;
    for (int i = 0; i < m and cnt < n - 1; ++i) {
        int u = edges[i].u, v = edges[i].v, w = edges[i].w;
        if (Find(u) not_eq Find(v)) {
            Union(u, v);
            ++cnt;
            sum += w;
        }
    }
    return cnt == n - 1 ? sum : -1;
}

int main()
{
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
#endif
    int n = read(), m = read();
    for (int i = 1; i <= n; ++i) {
        pre[i] = i;
    }
    for (int i = 0; i < m; ++i) {
        edges[i].u = read();
        edges[i].v = read();
        edges[i].w = read();
    }
    sort(edges, edges + m, [&](const edge& a, const edge& b) {
        return a.w < b.w;
    });
    int res = kruskal(n, m);
    if (compl res) {
        write(res, true);
    } else {
        puts("orz");
    }
    return 0;
}
```

#### Prim 算法

[洛谷 P3366 【模板】最小生成树](https://www.luogu.com.cn/problem/P3366)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int inf(0x3f3f3f3f);
const int maxn(5e3 + 10);
const int maxm(4e5 + 10);
int ecnt, head[maxn];
int dis[maxn];
bool vis[maxn];

struct edge {
    int to, wt, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' or ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' and ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline void addEdge(int u, int v, int w)
{
    edges[ecnt].to = v;
    edges[ecnt].wt = w;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

int prim(int n, int m)
{
    memset(dis, 0x3f, sizeof dis);
    dis[1] = 0;
    priority_queue<p, vector<p>, greater<p>> q;
    for (int i = 1; i <= n; ++i) {
        q.push(p(dis[i], i));
    }
    int sum = 0;
    while (not q.empty()) {
        int u = q.top().second;
        q.pop();
        if (vis[u]) continue;
        vis[u] = true;
        if (dis[u] == inf) return -1;
        sum += dis[u];
        for (int i = head[u]; compl i; i = edges[i].nxt) {
            int v = edges[i].to, w = edges[i].wt;
            if (dis[v] > w) {
                dis[v] = w;
                q.push(p(dis[v], v));
            }
        }
    }
    return sum;
}

int main()
{
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
#endif
    memset(head, -1, sizeof head);
    int n = read(), m = read();
    while (m--) {
        int u = read(), v = read(), w = read();
        addEdge(u, v, w);
        addEdge(v, u, w);
    }
    int res = prim(n, m);
    if (compl res) {
        write(res, true);
    } else {
        puts("orz");
    }
    return 0;
}
```

#### 倍增

[洛谷 P3379 【模板】最近公共祖先（LCA）](https://www.luogu.com.cn/problem/P3379)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(5e5 + 10);
const int maxm(1e6 + 10);
int ecnt, head[maxn];
int dep[maxn], f[maxn][20];

struct edge {
    int to, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

void addEdge(int u, int v)
{
    edges[ecnt].to = v;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

void bfs(int root)
{
    queue<int> q;
    q.push(root);
    dep[root] = 1;
    while (not q.empty()) {
        int u = q.front();
        q.pop();
        for (int i = head[u]; compl i; i = edges[i].nxt) {
            int v = edges[i].to;
            if (dep[v]) {
                continue;
            }
            q.push(v);
            dep[v] = dep[u] + 1;
            f[v][0] = u;
            for (int j = 1; j < 20; ++j) {
                f[v][j] = f[f[v][j - 1]][j - 1];
            }
        }
    }
}

int lca(int u, int v)
{
    if (dep[u] < dep[v]) {
        swap(u, v);
    }
    for (int i = 19; i >= 0; --i) {
        if (dep[f[u][i]] >= dep[v]) {
            u = f[u][i];
        }
    }
    if (u == v) {
        return u;
    }
    for (int i = 19; i >= 0; --i) {
        if (f[u][i] not_eq f[v][i]) {
            u = f[u][i];
            v = f[v][i];
        }
    }
    return f[u][0];
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    ios::sync_with_stdio(false);
    memset(head, -1, sizeof head);
    int n = read(), m = read(), s = read();
    for (int i = 0; i < n - 1; ++i) {
        int u = read(), v = read();
        addEdge(u, v);
        addEdge(v, u);
    }
    bfs(s);
    while (m--) {
        int u = read(), v = read();
        write(lca(u, v), true);
    }
    return 0;
}
```

[洛谷 P4180 [BJWC2010]严格次小生成树](https://www.luogu.com.cn/problem/P4180)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const int maxn(1e6 + 10);
const int maxm(3e6 + 10);
int ecnt, head[maxn];
int pre[maxn], dep[maxn];
int f[maxn][17], d1[maxn][17], d2[maxn][17];

struct edge {
    int u, v, w;
    bool used;
    
    bool operator <(const edge& rhs) const
    {
        return w < rhs.w;
    }
} edges[maxm];

struct _edge {
    int to, wt, nxt;
} _edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

void addEdge(int u, int v, int w)
{
    _edges[ecnt].to = v;
    _edges[ecnt].wt = w;
    _edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

int Find(int x)
{
    return pre[x] == x ? x : pre[x] = Find(pre[x]);
}

void Union(int u, int v)
{
    pre[Find(v)] = Find(u);
}

ll kruskal(int n, int m)
{
    for (int i = 1; i <= n; ++i) {
        pre[i] = i;
    }
    sort(edges, edges + m);
    ll res = 0;
    for (int i = 0; i < m; ++i) {
        int u = edges[i].u, v = edges[i].v, w = edges[i].w;
        if (Find(u) != Find(v)) {
            Union(u, v);
            res += w;
            edges[i].used = true;
        }
    }
    return res;
}

void build(int m)
{
    memset(head, -1, sizeof head);
    for (int i = 0; i < m; ++i) {
        if (edges[i].used) {
            int u = edges[i].u, v = edges[i].v, w = edges[i].w;
            addEdge(u, v, w);
            addEdge(v, u, w);
        }
    }
}

void bfs(int root)
{
    dep[root] = 1;
    queue<int> q;
    q.push(root);
    while (not q.empty()) {
        int u = q.front();
        q.pop();
        for (int i = head[u]; compl i; i = _edges[i].nxt) {
            int v = _edges[i].to, w = _edges[i].wt;
            if (dep[v]) {
                continue;
            }
            dep[v] = dep[u] + 1;
            q.push(v);
            f[v][0] = u;
            d1[v][0] = w;
            d2[v][0] = -inf;
            for (int j = 1; j < 17; ++j) {
                int anc = f[v][j - 1];
                f[v][j] = f[anc][j - 1];
                int dis[4] = { d1[v][j - 1], d2[v][j - 1], d1[anc][j - 1], d2[anc][j - 1] };
                d1[v][j] = d2[v][j] = -inf;
                for (int k = 0; k < 4; ++k) {
                    int d = dis[k];
                    if (d > d1[v][j]) {
                        d2[v][j] = d1[v][j];
                        d1[v][j] = d;
                    } else if (d not_eq d1[v][j] and d > d2[v][j]) {
                        d2[v][j] = d;
                    }
                }
            }
        }
    }
}

int lca(int u, int v, int w)
{
    static int dis[maxn * 2];
    int cnt = 0;
    if (dep[u] < dep[v]) {
        swap(u, v);
    }
    for (int i = 16; i >= 0; --i) {
        if (dep[f[u][i]] >= dep[v]) {
            dis[cnt++] = d1[u][i];
            dis[cnt++] = d2[u][i];
            u = f[u][i];
        }
    }
    if (u not_eq v) {
        for (int i = 16; i >= 0; --i) {
            if (f[u][i] not_eq f[v][i]) {
                dis[cnt++] = d1[u][i];
                dis[cnt++] = d2[u][i];
                dis[cnt++] = d1[v][i];
                dis[cnt++] = d2[v][i];
                u = f[u][i];
                v = f[v][i];
            }
        }
        dis[cnt++] = d1[u][0];
        dis[cnt++] = d1[v][0];
    }
    int dis1 = -inf, dis2 = -inf;
    for (int i = 0; i < cnt; ++i) {
        int d = dis[i];
        if (d > dis1) {
            dis2 = dis1;
            dis1 = d;
        } else if (d not_eq dis1 and d > dis2) {
            dis2 = d;
        }
    }
    if (w > dis1) {
        return w - dis1;
    }
    if (w > dis2) {
        return w - dis2;
    }
    return inf;
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    ios::sync_with_stdio(false);
    int n = read(), m = read();
    for (int i = 0; i < m; ++i) {
        int u = read(), v = read(), w = read();
        addEdge(u, v, w);
        edges[i] = edge{ u, v, w };
    }
    ll sum = kruskal(n, m);
    build(m);
    bfs(1);
    ll res = 1e18;
    for (int i = 0; i < m; ++i) {
        if (not edges[i].used) {
            int u = edges[i].u, v = edges[i].v, w = edges[i].w;
            res = min(res, sum + lca(u, v, w));
        }
    }
    write(res, true);
    return 0;
}
```

#### 离线 Tarjan 算法

[洛谷 P3379 【模板】最近公共祖先（LCA）](https://www.luogu.com.cn/problem/P3379)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(5e5 + 10);
const int maxm(1e6 + 10);
int ecnt, qcnt;
bool vis[maxn];
int ehead[maxn], qhead[maxm];
int pre[maxn], ans[maxm];

struct edge {
    int to, nxt;
} edges[maxm];

struct que {
    int id, to, nxt;
} ques[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

void addEdge(int u, int v)
{
    edges[ecnt].to = v;
    edges[ecnt].nxt = ehead[u];
    ehead[u] = ecnt++;
}

void addQue(int id, int u, int v)
{
    ques[qcnt].id = id;
    ques[qcnt].to = v;
    ques[qcnt].nxt = qhead[u];
    qhead[u] = qcnt++;
}

int Find(int x)
{
    return pre[x] == x ? x : pre[x] = Find(pre[x]);
}

void Union(int u, int v)
{
    pre[Find(v)] = Find(u);
}

void dfs(int cur, int pre)
{
    vis[cur] = true;
    for (int i = ehead[cur]; compl i; i = edges[i].nxt) {
        int nxt = edges[i].to;
        if (nxt not_eq pre) {
            dfs(nxt, cur);
        }
    }
    for (int i = qhead[cur]; compl i; i = ques[i].nxt) {
        int id = ques[i].id, nxt = ques[i].to;
        if (vis[nxt]) {
            ans[id] = Find(nxt);
        }
    }
    Union(pre, cur);
}

void tarjan(int tot, int root)
{
    for (int i = 1; i <= tot; ++i) {
        pre[i] = i;
    }
    dfs(root, 0);
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    ios::sync_with_stdio(false);
    memset(ehead, -1, sizeof ehead);
    memset(qhead, -1, sizeof qhead);
    int n = read(), m = read(), s = read();
    for (int i = 0; i < n - 1; ++i) {
        int u = read(), v = read();
        addEdge(u, v);
        addEdge(v, u);
    }
    for (int i = 0; i < m; ++i) {
        int u = read(), v = read();
        addQue(i, u, v);
        addQue(i, v, u);
    }
    tarjan(n, s);
    for (int i = 0; i < m; ++i) {
        write(ans[i], true);
    }
    return 0;
}
```

#### Tarjan 算法

[洛谷 P3388 【模板】割点（割顶）](https://www.luogu.com.cn/problem/P3388)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const int mod(1e9 + 7);
const int maxn(2e4 + 10);
const int maxm(2e5 + 10);
int ecnt, head[maxn];
int tim, dfn[maxn], low[maxn];
int ccnt;
bool cut[maxn];

struct edge {
    int to, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

void addEdge(int u, int v)
{
    edges[ecnt].to = v;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

void tarjan(int cur, int pre)
{
    int child = 0;
    dfn[cur] = low[cur] = ++tim;
    for (int i = head[cur]; compl i; i = edges[i].nxt) {
        int nxt = edges[i].to;
        if (not dfn[nxt]) {
            ++child;
            tarjan(nxt, cur);
            low[cur] = min(low[cur], low[nxt]);
            if (pre not_eq cur and low[nxt] >= dfn[cur] and not cut[cur]) {
                ++ccnt;
                cut[cur] = true;
            }
        } else if (nxt not_eq pre) {
            low[cur] = min(low[cur], dfn[nxt]);
        }
    }
    if (pre == cur and child >= 2) {
        ++ccnt;
        cut[cur] = true;
    }
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    ios::sync_with_stdio(false);
    memset(head, -1, sizeof head);
    int n = read(), m = read();
    while (m--) {
        int u = read(), v = read();
        addEdge(u, v);
        addEdge(v, u);
    }
    for (int i = 1; i <= n; ++i) {
        if (not dfn[i]) {
            tarjan(i, i);
        }
    }
    write(ccnt, true);
    for (int i = 1; i <= n; ++i) {
        if (cut[i]) {
            write(i, false);
            putchar(32);
        }
    }
    return 0;
}
```

[洛谷 P3387 【模板】缩点](https://www.luogu.com.cn/problem/P3387)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const int mod(1e9 + 7);
const int maxn(1e4 + 10);
const int maxm(2e5 + 10);
int ecnt, head[maxn], shead[maxn];
int tim, dfn[maxn], low[maxn];
int scnt, scc[maxn], siz[maxn];
int w[maxn], sum[maxn], deg[maxn];
bool vis[maxn];
stack<int> st;

struct edge {
    int to, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

void addEdge(int u, int v, bool s = false)
{
    edges[ecnt].to = v;
    if (s) {
        edges[ecnt].nxt = shead[u];
        shead[u] = ecnt++;
    } else {
        edges[ecnt].nxt = head[u];
        head[u] = ecnt++;
    }
}

void tarjan(int u)
{
    dfn[u] = low[u] = ++tim;
    st.push(u);
    vis[u] = true;
    for (int i = head[u]; compl i; i = edges[i].nxt) {
        int v = edges[i].to;
        if (not dfn[v]) {
            tarjan(v);
            low[u] = min(low[u], low[v]);
        } else if (vis[v]) {
            low[u] = min(low[u], dfn[v]);
        }
    }
    if (dfn[u] == low[u]) {
        ++scnt;
        int v = 0;
        do {
            v = st.top();
            st.pop();
            vis[v] = false;
            scc[v] = scnt;
            ++siz[scnt];
            sum[scnt] += w[v];
        } while (v not_eq u);
    }
}

int dfs(int u)
{
    int mx = 0;
    for (int i = shead[u]; compl i; i = edges[i].nxt) {
        int v = edges[i].to;
        mx = max(mx, dfs(v));
    }
    return sum[u] + mx;
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    memset(head, -1, sizeof head);
    memset(shead, -1, sizeof shead);
    int n = read(), m = read();
    for (int i = 1; i <= n; ++i) {
        w[i] = read();
    }
    while (m--) {
        int u = read(), v = read();
        addEdge(u, v);
    }
    for (int i = 1; i <= n; ++i) {
        if (not dfn[i]) {
            tarjan(i);
        }
    }
    for (int u = 1; u <= n; ++u) {
        for (int i = head[u]; compl i; i = edges[i].nxt) {
            int v = edges[i].to;
            int a = scc[u], b = scc[v];
            if (a not_eq b) {
                addEdge(a, b, true);
                ++deg[b];
            }
        }
    }
    int res = 0;
    for (int i = 1; i <= scnt; ++i) {
        if (not deg[i]) {
            res = max(res, dfs(i));
        }
    }
    write(res, true);
    return 0;
}
```

[洛谷 P4782 【模板】2-SAT 问题](https://www.luogu.com.cn/problem/P4782)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const int mod(1e9 + 7);
const int maxn(2e6 + 10);
const int maxm(2e6 + 10);
int ecnt, head[maxn];
int tim, dfn[maxn], low[maxn];
int scnt, id[maxn];
bool vis[maxn];
stack<int> st;

struct edge {
    int to, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, char c = false)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (c) putchar(c);
}

void addEdge(int u, int v)
{
    edges[ecnt].to = v;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

void tarjan(int u)
{
    dfn[u] = low[u] = ++tim;
    st.push(u);
    vis[u] = true;
    for (int i = head[u]; compl i; i = edges[i].nxt) {
        int v = edges[i].to;
        if (not dfn[v]) {
            tarjan(v);
            low[u] = min(low[u], low[v]);
        } else if (vis[v]) {
            low[u] = min(low[u], dfn[v]);
        }
    }
    if (dfn[u] == low[u]) {
        int v = -1;
        ++scnt;
        do {
            v = st.top();
            st.pop();
            vis[v] = false;
            id[v] = scnt;
        } while (u not_eq v);
    }
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    int n = read(), m = read();
    memset(head, -1, sizeof head);
    while (m--) {
        int u = read() - 1, a = read(), v = read() - 1, b = read();
        addEdge(u * 2 + not a, v * 2 + b);
        addEdge(v * 2 + not b, u * 2 + a);
    }
    for (int i = 0; i < n * 2; ++i) {
        if (not dfn[i]) {
            tarjan(i);
        }
    }
    bool flag = false;
    for (int i = 0; i < n; ++i) {
        if (id[i * 2] == id[i * 2 + 1]) {
            flag = true;
            break;
        }
    }
    if (flag) {
        puts("IMPOSSIBLE");
    } else {
        puts("POSSIBLE");
        for (int i = 0; i < n; ++i) {
            if (id[i * 2] < id[i * 2 + 1]) {
                write(0, ' ');
            } else {
                write(1, ' ');
            }
        }
    }
    return 0;
}
```

#### Hungary 算法

[洛谷 P3386 【模板】二分图最大匹配](https://www.luogu.com.cn/problem/P3386)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const int mod(1e9 + 7);
const int maxn(1e3 + 10);
const int maxm(1e5 + 10);
int ecnt, head[maxn];
bool vis[maxn];
int match[maxn];

struct edge {
    int to, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

void addEdge(int u, int v)
{
    edges[ecnt].to = v;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

bool dfs(int u)
{
    for (int i = head[u]; compl i; i = edges[i].nxt) {
        int v = edges[i].to;
        if (not vis[v]) {
            vis[v] = true;
            if (not match[v] or dfs(match[v])) {
                match[v] = u;
                return true;
            }
        }
    }
    return false;
}

int hungary(int n)
{
    int res = 0;
    for (int i = 1; i <= n; ++i) {
        memset(vis, false, sizeof vis);
        res += dfs(i);
    }
    return res;
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    ios::sync_with_stdio(false);
    memset(head, -1, sizeof head);
    int n = read(), m = read(), e = read();
    while (e--) {
        int u = read(), v = read() + n;
        addEdge(u, v);
        addEdge(v, u);
    }
    write(hungary(n), true);
    return 0;
}
```

#### A* 算法

[POJ 2449 Remmarguts' Date](http://poj.org/problem?id=2449)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(1e3 + 10);
const int maxm(2e5 + 10);
int ecnt, head[maxn], rhead[maxn];
bool vis[maxn];
int g[maxn];

struct edge {
    int to, wt, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' or ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' and ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline void addEdge(int u, int v, int w, bool r)
{
    edges[ecnt].to = v;
    edges[ecnt].wt = w;
    if (r) {
        edges[ecnt].nxt = rhead[u];
        rhead[u] = ecnt++;
    } else {
        edges[ecnt].nxt = head[u];
        head[u] = ecnt++;
    }
}

void dijkstra(int src)
{
    memset(g, 0x3f, sizeof g);
    g[src] = 0;
    priority_queue<p, vector<p>, greater<p>> q;
    q.push(p(0, src));
    while (not q.empty()) {
        int u = q.top().second;
        q.pop();
        if (vis[u]) continue;
        vis[u] = true;
        for (int i = rhead[u]; compl i; i = edges[i].nxt) {
            int v = edges[i].to, w = edges[i].wt;
            if (g[v] > g[u] + w) {
                g[v] = g[u] + w;
                q.push(p(g[v], v));
            }
        }
    }
}

int astar(int src, int des, int k)
{
    if (src == des) ++k;
    auto cmp = [&](const p& a, const p& b) {
        return a.second + g[a.first] > b.second + g[b.first];
    };
    priority_queue<p, vector<p>, decltype(cmp)> q(cmp);
    q.push(p(src, 0));
    int cnt = 0;
    while (not q.empty()) {
        int u = q.top().first, d = q.top().second;
        q.pop();
        if (u == des and ++cnt == k) {
            return d;
        }
        for (int i = head[u]; compl i; i = edges[i].nxt) {
            int v = edges[i].to, w = edges[i].wt;
            q.push(p(v, d + w));
        }
    }
    return -1;
}

int main()
{
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
#endif
    memset(head, -1, sizeof head);
    memset(rhead, -1, sizeof rhead);
    int n = read(), m = read();
    while (m--) {
        int u = read(), v = read(), w = read();
        addEdge(u, v, w, false);
        addEdge(v, u, w, true);
    }
    int s = read(), t = read(), k = read();
    dijkstra(t);
    write(astar(s, t, k), true);
    return 0;
}
```

#### EK 算法

[洛谷 P3376 【模板】网络最大流](https://www.luogu.com.cn/problem/P3376)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int inf(0x3f3f3f3f);
const int maxn(1e3 + 10);
const int maxm(2e4 + 10);
int ecnt, head[maxn];
int mn[maxn], pre[maxn];
bool vis[maxn];

struct edge {
    int to, flow, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' or ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' and ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline void addEdge(int u, int v, int w)
{
    edges[ecnt].to = v;
    edges[ecnt].flow = w;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

bool bfs(int src, int des)
{
    queue<int> q;
    memset(vis, false, sizeof vis);
    q.push(src);
    vis[src] = true;
    mn[src] = inf;
    while (not q.empty()) {
        int u = q.front();
        q.pop();
        for (int i = head[u]; compl i; i = edges[i].nxt) {
            int v = edges[i].to, w = edges[i].flow;
            if (not vis[v] and w) {
                vis[v] = true;
                mn[v] = min(mn[u], w);
                pre[v] = i;
                if (v == des) {
                    return true;
                }
                q.push(v);
            }
        }
    }
    return false;
}

ll ek(int src, int des)
{
    ll res = 0;
    while (bfs(src, des)) {
        res += mn[des];
        for (int i = des; i not_eq src; i = edges[pre[i] xor 1].to) {
            edges[pre[i]].flow -= mn[des];
            edges[pre[i] xor 1].flow += des;
        }
    }
    return res;
}

int main()
{
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
#endif
    memset(head, -1, sizeof head);
    int n = read(), m = read(), s = read(), t = read();
    while (m--) {
        int u = read(), v = read(), w = read();
        addEdge(u, v, w);
        addEdge(v, u, 0);
    }
    write(ek(s, t), true);
    return 0;
}
```

#### Dinic 算法

[洛谷 P3376 【模板】网络最大流](https://www.luogu.com.cn/problem/P3376)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int inf(0x3f3f3f3f);
const int maxn(1e4 + 10);
const int maxm(2e5 + 10);
int ecnt, head[maxn];
int dep[maxn], cur[maxn];

struct edge {
    int to, flow, nxt;
} edges[maxm];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' or ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' and ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline void addEdge(int u, int v, int w)
{
    edges[ecnt].to = v;
    edges[ecnt].flow = w;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

bool bfs(int src, int des)
{
    queue<int> q;
    memset(dep, -1, sizeof(dep));
    dep[src] = 0;
    q.push(src);
    while (not q.empty()) {
        int u = q.front();
        q.pop();
        for (int i = head[u]; compl i; i = edges[i].nxt) {
            int v = edges[i].to, w = edges[i].flow;
            if (dep[v] == -1 and w) {
                q.push(v);
                dep[v] = dep[u] + 1;
            }
        }
    }
    return compl dep[des];
}

int dfs(int u, int des, int rflow)
{
    if (u == des) return rflow;
    int res = 0;
    for (int i = cur[u]; compl i and rflow; i = edges[i].nxt) {
        int v = edges[i].to;
        if (dep[v] not_eq dep[u] + 1 or not edges[i].flow) continue;
        cur[u] = i;
        int flow = dfs(v, des, min(edges[i].flow, rflow));
        edges[i].flow -= flow;
        edges[i xor 1].flow += flow;
        rflow -= flow;
        res += flow;
    }
    if (not res) dep[u] = -1;
    return res;
}

ll dinic(int src, int des)
{
    ll res = 0;
    while (bfs(src, des)) {
        memcpy(cur, head, sizeof(head));
        res += dfs(src, des, inf);
    }
    return res;
}

int main()
{
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
#endif
    int n = read(), m = read(), s = read(), t = read();
    memset(head, -1, sizeof(head));
    while (m--) {
        int u = read(), v = read(), w = read();
        addEdge(u, v, w);
        addEdge(v, u, 0);
    }
    write(dinic(s, t), true);
    return 0;
}
```

## 数据结构

#### 线段树

[洛谷 P3372 【模板】线段树 1](https://www.luogu.com.cn/problem/P3372)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(1e5 + 10);

struct node {
    int l, r;
    ll sum, lz;
} tree[maxn << 2];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline int ls(int cur)
{
    return cur << 1;
}

inline int rs(int cur)
{
    return cur << 1 | 1;
}

void push_up(int cur)
{
    tree[cur].sum = tree[ls(cur)].sum + tree[rs(cur)].sum;
}

void push_down(int cur)
{
    if (tree[cur].lz) {
        tree[ls(cur)].sum += (tree[ls(cur)].r - tree[ls(cur)].l + 1) * tree[cur].lz;
        tree[rs(cur)].sum += (tree[rs(cur)].r - tree[rs(cur)].l + 1) * tree[cur].lz;
        tree[ls(cur)].lz += tree[cur].lz;
        tree[rs(cur)].lz += tree[cur].lz;
        tree[cur].lz = 0;
    }
}

void build(int cur, int l, int r)
{
    tree[cur].l = l;
    tree[cur].r = r;
    tree[cur].lz = 0;
    if (l == r) {
        tree[cur].sum = read();
        return;
    }
    int mid = (l + r) >> 1;
    build(ls(cur), l, mid);
    build(rs(cur), mid + 1, r);
    push_up(cur);
}

void update(int cur, int l, int r, int v)
{
    if (tree[cur].l == l and tree[cur].r == r) {
        tree[cur].sum += (r - l + 1) * v;
        tree[cur].lz += v;
        return;
    }
    push_down(cur);
    int mid = (tree[cur].l + tree[cur].r) >> 1;
    if (r <= mid) {
        update(ls(cur), l, r, v);
    } else if (l > mid) {
        update(rs(cur), l, r, v);
    } else {
        update(ls(cur), l, mid, v);
        update(rs(cur), mid + 1, r, v);
    }
    push_up(cur);
}

ll query(int cur, int l, int r)
{
    if (tree[cur].l == l and tree[cur].r == r) {
        return tree[cur].sum;
    }
    push_down(cur);
    int mid = (tree[cur].l + tree[cur].r) >> 1;
    if (r <= mid) return query(ls(cur), l, r);
    if (l > mid) return query(rs(cur), l, r);
    return query(ls(cur), l, mid) + query(rs(cur), mid + 1, r);
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    int n = read(), m = read();
    build(1, 1, n);
    while (m--) {
        int t = read(), x = read(), y = read();
        if (t == 1) {
            int k = read();
            update(1, x, y, k);
        } else {
            write(query(1, x, y), true);
        }
    }
    return 0;
}
```

[洛谷 P3373 【模板】线段树 2](https://www.luogu.com.cn/problem/P3373)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(1e5 + 10);
int mod;

struct {
    int l, r;
    ll val, add, mult;
} tree[maxn << 2];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline int ls(int cur)
{
    return cur << 1;
}

inline int rs(int cur)
{
    return cur << 1 | 1;
}

void push_up(int cur)
{
    tree[cur].val = (tree[ls(cur)].val + tree[rs(cur)].val) % mod;
}

void push_down(int cur)
{
    if (tree[cur].add or tree[cur].mult not_eq 1) {
        tree[ls(cur)].val = ((tree[ls(cur)]).val * tree[cur].mult + (tree[ls(cur)].r - tree[ls(cur)].l + 1) * tree[cur].add) % mod;
        tree[rs(cur)].val = ((tree[rs(cur)]).val * tree[cur].mult + (tree[rs(cur)].r - tree[rs(cur)].l + 1) * tree[cur].add) % mod;
        tree[ls(cur)].mult = tree[ls(cur)].mult * tree[cur].mult % mod;
        tree[rs(cur)].mult = tree[rs(cur)].mult * tree[cur].mult % mod;
        tree[ls(cur)].add = (tree[ls(cur)].add * tree[cur].mult + tree[cur].add) % mod;
        tree[rs(cur)].add = (tree[rs(cur)].add * tree[cur].mult + tree[cur].add) % mod;
        tree[cur].add = 0;
        tree[cur].mult = 1;
    }
}

void build(int cur, int l, int r)
{
    tree[cur].l = l;
    tree[cur].r = r;
    tree[cur].mult = 1;
    if (l == r) {
        tree[cur].val = read();
        return;
    }
    int mid = (l + r) >> 1;
    build(ls(cur), l, mid);
    build(rs(cur), mid + 1, r);
    push_up(cur);
}

void update(int cur, int l, int r, int op, int v)
{
    if (tree[cur].l == l and tree[cur].r == r) {
        if (op == 1) {
            tree[cur].val = tree[cur].val * v % mod;
            tree[cur].add = tree[cur].add * v % mod;
            tree[cur].mult = tree[cur].mult * v % mod;
        } else {
            tree[cur].val = (tree[cur].val + (r - l + 1) * v) % mod;
            tree[cur].add = (tree[cur].add + v) % mod;
        }
        return;
    }
    push_down(cur);
    int mid = (tree[cur].l + tree[cur].r) >> 1;
    if (r <= mid) {
        update(ls(cur), l, r, op, v);
    } else if (l > mid) {
        update(rs(cur), l, r, op, v);
    } else {
        update(ls(cur), l, mid, op, v);
        update(rs(cur), mid + 1, r, op, v);
    }
    push_up(cur);
}

ll query(int cur, int l, int r)
{
    if (tree[cur].l == l and tree[cur].r == r) {
        return tree[cur].val;
    }
    push_down(cur);
    int mid = (tree[cur].l + tree[cur].r) >> 1;
    if (r <= mid) {
        return query(ls(cur), l, r);
    }
    if (l > mid) {
        return query(rs(cur), l, r);
    }
    return (query(ls(cur), l, mid) + query(rs(cur), mid + 1, r)) % mod;
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    int n = read(), m = read();
    mod = read();
    build(1, 1, n);
    while (m--) {
        int op = read(), x = read(), y = read();
        if (op == 3) {
            write(query(1, x, y), true);
        } else {
            update(1, x, y, op, read());
        }
    }
    return 0;
}
```

#### 树状数组

[洛谷 P3374 【模板】树状数组 1](https://www.luogu.com.cn/problem/P3374)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(5e5 + 10);
int c[maxn];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline int lowbit(int x)
{
    return x & -x;
}

void update(int p, int n, int v)
{
    for (int i = p; i <= n; i += lowbit(i)) {
        c[i] += v;
    }
}

ll getSum(int p)
{
    ll sum = 0;
    while (p) {
        sum += c[p];
        p -= lowbit(p);
    }
    return sum;
}

ll query(int l, int r)
{
    return getSum(r) - getSum(l - 1);
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    ios::sync_with_stdio(false);
    int n = read(), m = read();
    for (int i = 1; i <= n; ++i) {
        update(i, n, read());
    }
    while (m--) {
        int op = read(), a = read(), b = read();
        if (op == 1) {
            update(a, n, b);
        } else {
            write(query(a, b), true);
        }
    }
    return 0;
}
```

[洛谷 P3368 【模板】树状数组 2](https://www.luogu.com.cn/problem/P3368)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(5e5 + 10);
int c[maxn];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline int lowbit(int x)
{
    return x & -x;
}

void update(int p, int n, int v)
{
    for (int i = p; i <= n; i += lowbit(i)) {
        c[i] += v;
    }
}

ll getSum(int p)
{
    ll sum = 0;
    while (p) {
        sum += c[p];
        p -= lowbit(p);
    }
    return sum;
}

ll query(int l, int r)
{
    return getSum(r) - getSum(l - 1);
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    ios::sync_with_stdio(false);
    int n = read(), m = read();
    for (int i = 1; i <= n; ++i) {
        update(i, n, read());
    }
    while (m--) {
        int op = read(), a = read(), b = read();
        if (op == 1) {
            update(a, n, b);
        } else {
            write(query(a, b), true);
        }
    }
    return 0;
}
```

[洛谷 P3372 【模板】线段树 1](https://www.luogu.com.cn/problem/P3372)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(1e5 + 10);
int a[maxn];
ll c[maxn][2];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline int lowbit(int x)
{
    return x & -x;
}

void add(int t, int p, int n, ll v)
{
    for (int i = p; i <= n; i += lowbit(i)) {
        c[i][t] += v;
    }
}

ll getSum(int t, int p)
{
    ll sum = 0;
    for (int i = p; i; i -= lowbit(i)) {
        sum += c[i][t];
    }
    return sum;
}

void update(int l, int r, int n, int v)
{
    add(0, l, n, v);
    add(0, r + 1, n, -v);
    add(1, l, n, 1ll * v * (l - 1));
    add(1, r + 1, n, -1ll * v * r);
}

ll query(int l, int r)
{
    ll sum_r = 1ll * r * getSum(0, r) - getSum(1, r);
    ll sum_l = 1ll * (l - 1) * getSum(0, l - 1) - getSum(1, l - 1);
    return sum_r - sum_l;
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    int n = read(), m = read();
    for (int i = 1; i <= n; ++i) {
        a[i] = read();
        add(0, i, n, a[i] - a[i - 1]);
        add(1, i, n, 1ll * (a[i] - a[i - 1]) * (i - 1));
    }
    while (m--) {
        int op = read(), x = read(), y = read();
        if (op == 1) {
            int k = read();
            update(x, y, n, k);
        } else {
            write(query(x, y), true);
        }
    }
    return 0;
}
```

#### 可持久化数组

[洛谷 P3919 【模板】可持久化线段树 1（可持久化数组）](https://www.luogu.com.cn/problem/P3919)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const int mod(1e9 + 7);
const int maxn(1e6 + 10);
int cnt, a[maxn], root[maxn];

struct node {
    int val, l, r;
} tree[maxn * 40];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

void build(int l, int r, int& cur)
{
    cur = ++cnt;
    if (l == r) {
        tree[cur].val = a[l];
        return;
    }
    int mid = (l + r) >> 1;
    build(l, mid, tree[cur].l);
    build(mid + 1, r, tree[cur].r);
}

void update(int l, int r, int pos, int val, int pre, int& cur)
{
    tree[cur = ++cnt] = tree[pre];
    if (l == r) {
        tree[cur].val = val;
        return;
    }
    int mid = (l + r) >> 1;
    if (pos <= mid) {
        update(l, mid, pos, val, tree[pre].l, tree[cur].l);
    } else {
        update(mid + 1, r, pos, val, tree[pre].r, tree[cur].r);
    }
}

int query(int l, int r, int pos, int cur)
{
    if (l == r) {
        return tree[cur].val;
    }
    int mid = (l + r) >> 1;
    if (pos <= mid) {
        return query(l, mid, pos, tree[cur].l);
    }
    return query(mid + 1, r, pos, tree[cur].r);
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    ios::sync_with_stdio(false);
    int n = read(), m = read();
    for (int i = 1; i <= n; ++i) {
        a[i] = read();
    }
    build(1, n, root[0]);
    for (int i = 1; i <= m; ++i) {
        int v = read(), op = read(), pos = read();
        if (op == 1) {
            int val = read();
            update(1, n, pos, val, root[v], root[i]);
        } else {
            write(query(1, n, pos, root[i] = root[v]), true);
        }
    }
    return 0;
}
```

```cpp
#include <bits/stdc++.h>
#include <ext/rope>

using namespace std;
using namespace __gnu_cxx;
const int maxn(1e6 + 10);
rope<int> rp[maxn];

int main()
{
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
#endif
    ios::sync_with_stdio(false);
    int n, m;
    cin >> n >> m;
    rp[0].append(0);
    for (int i = 1; i <= n; ++i) {
        int x;
        cin >> x;
        rp[0].append(x);
    }
    for (int i = 1; i <= m; ++i) {
        int v, op, pos, val;
        cin >> v >> op >> pos;
        rp[i] = rp[v];
        if (op == 1) {
            cin >> val;
            rp[i].replace(pos, val);
        } else {
            cout << rp[i][pos] << endl;
        }
    }
    return 0;
}
```

#### 主席树

[洛谷 P3834 【模板】可持久化线段树 2（主席树）](https://www.luogu.com.cn/problem/P3834)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const int mod(1e9 + 7);
const int maxn(2e5 + 10);
int cnt, a[maxn], root[maxn];
vector<int> nums;

struct node {
    int l, r, sum;
} tree[maxn * 40];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline int getId(int val)
{
    return int(lower_bound(nums.begin(), nums.end(), val) - nums.begin()) + 1;
}

void insert(int l, int r, int pos, int pre, int& cur)
{
    cur = ++cnt;
    tree[cur] = tree[pre];
    ++tree[cur].sum;
    if (l == r) return;
    int mid = (l + r) >> 1;
    if (pos <= mid) {
        insert(l, mid, pos, tree[pre].l, tree[cur].l);
    } else {
        insert(mid + 1, r, pos, tree[pre].r, tree[cur].r);
    }
}

int query(int l, int r, int k, int pre, int cur)
{
    if (l == r) return l;
    int mid = (l + r) >> 1;
    int dif = tree[tree[cur].l].sum - tree[tree[pre].l].sum;
    if (k <= dif) {
        return query(l, mid, k, tree[pre].l, tree[cur].l);
    }
    return query(mid + 1, r, k - dif, tree[pre].r, tree[cur].r);
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    ios::sync_with_stdio(false);
    int n = read(), m = read();
    for (int i = 1; i <= n; ++i) {
        nums.push_back(a[i] = read());
    }
    sort(nums.begin(), nums.end());
    nums.erase(unique(nums.begin(), nums.end()), nums.end());
    for (int i = 1; i <= n; ++i) {
        int pos = getId(a[i]);
        insert(1, (int)nums.size(), pos, root[i - 1], root[i]);
    }
    while (m--) {
        int l = read(), r = read(), k = read();
        write(nums[query(1, (int)nums.size(), k, root[l - 1], root[r]) - 1], true);
    }
    return 0;
}
```

#### 树堆（Treap）

[洛谷 P3369 【模板】普通平衡树](https://www.luogu.com.cn/problem/P3369)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(1e5 + 10);
const int inf(0x3f3f3f3f);
int idx, root;

struct node {
    int l, r;
    int key, val;
    int cnt, siz;
} tree[maxn];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline int new_node(int key)
{
    ++idx;
    tree[idx].key = key;
    tree[idx].val = rand();
    tree[idx].cnt = tree[idx].siz = 1;
    return idx;
}

inline void push_up(int cur)
{
    tree[cur].siz = tree[tree[cur].l].siz + tree[tree[cur].r].siz + tree[cur].cnt;
}

inline void zig(int& cur)
{
    int ls = tree[cur].l;
    tree[cur].l = tree[ls].r;
    tree[ls].r = cur;
    cur = ls;
    push_up(tree[cur].r);
    push_up(cur);
}

inline void zag(int& cur)
{
    int rs = tree[cur].r;
    tree[cur].r = tree[rs].l;
    tree[rs].l = cur;
    cur = rs;
    push_up(tree[cur].l);
    push_up(cur);
}

void build()
{
    root = new_node(-inf);
    tree[root].r = new_node(inf);
    push_up(root);
    if (tree[root].val < tree[tree[root].r].val) {
        zag(root);
    }
}

inline void insert(int& cur, int key)
{
    if (not cur) {
        cur = new_node(key);
    } else if (key == tree[cur].key) {
        ++tree[cur].cnt;
    } else if (key < tree[cur].key) {
        insert(tree[cur].l, key);
        if (tree[tree[cur].l].val > tree[cur].val) {
            zig(cur);
        }
    } else {
        insert(tree[cur].r, key);
        if (tree[tree[cur].r].val > tree[cur].val) {
            zag(cur);
        }
    }
    push_up(cur);
}

inline void remove(int& cur, int key)
{
    if (not cur) return;
    if (key == tree[cur].key) {
        if (tree[cur].cnt > 1) {
            --tree[cur].cnt;
        } else if (tree[cur].l or tree[cur].r) {
            if (not tree[cur].r or tree[tree[cur].l].val > tree[tree[cur].r].val) {
                zig(cur);
                remove(tree[cur].r, key);
            } else {
                zag(cur);
                remove(tree[cur].l, key);
            }
        } else {
            cur = 0;
        }
    } else if (key < tree[cur].key) {
        remove(tree[cur].l, key);
    } else {
        remove(tree[cur].r, key);
    }
    push_up(cur);
}

inline int get_rank(int cur, int key)
{
    if (not cur) {
        return 0;
    }
    if (key == tree[cur].key) {
        return tree[tree[cur].l].siz + 1;
    }
    if (key < tree[cur].key) {
        return get_rank(tree[cur].l, key);
    }
    return get_rank(tree[cur].r, key) + tree[tree[cur].l].siz + tree[cur].cnt;
}

inline int get_key(int cur, int rank)
{
    if (not cur) {
        return inf;
    }
    if (rank <= tree[tree[cur].l].siz) {
        return get_key(tree[cur].l, rank);
    }
    if (rank <= tree[tree[cur].l].siz + tree[cur].cnt) {
        return tree[cur].key;
    }
    return get_key(tree[cur].r, rank - tree[cur].cnt - tree[tree[cur].l].siz);
}

inline int get_prev(int cur, int key)
{
    if (not cur) {
        return -inf;
    }
    if (key <= tree[cur].key) {
        return get_prev(tree[cur].l, key);
    }
    return max(tree[cur].key, get_prev(tree[cur].r, key));
}

inline int get_next(int cur, int key)
{
    if (not cur) {
        return inf;
    }
    if (key >= tree[cur].key) {
        return get_next(tree[cur].r, key);
    }
    return min(tree[cur].key, get_next(tree[cur].l, key));
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    build();
    int n = read();
    while (n--) {
        int op = read(), x = read();
        if (op == 1) {
            insert(root, x);
        } else if (op == 2) {
            remove(root, x);
        } else if (op == 3) {
            write(get_rank(root, x) - 1, true);
        } else if (op == 4) {
            write(get_key(root, x + 1), true);
        } else if (op == 5) {
            write(get_prev(root, x), true);
        } else {
            write(get_next(root, x), true);
        }
    }
    return 0;
}
```

#### 无旋树堆（fhq Treap）

[洛谷 P3369 【模板】普通平衡树](https://www.luogu.com.cn/problem/P3369)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int inf(0x3f3f3f3f);
const int maxn(1e5 + 10);
int idx, root;
int x, y, z;

struct node {
    int l, r;
    int key, val;
    int siz;
} tree[maxn];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline int new_node(int key)
{
    ++idx;
    tree[idx].key = key;
    tree[idx].val = rand();
    tree[idx].siz = 1;
    return idx;
}

inline void push_up(int cur)
{
    tree[cur].siz = tree[tree[cur].l].siz + tree[tree[cur].r].siz + 1;
}

inline void split(int cur, int key, int& x, int& y)
{
    if (not cur) {
        x = y = 0;
    } else if (tree[cur].key <= key) {
        x = cur;
        split(tree[cur].r, key, tree[cur].r, y);
        push_up(cur);
    } else {
        y = cur;
        split(tree[cur].l, key, x, tree[cur].l);
        push_up(cur);
    }
}

inline int merge(int x, int y)
{
    if (not x or not y) {
        return x + y;
    }
    if (tree[x].val > tree[y].val) {
        tree[x].r = merge(tree[x].r, y);
        push_up(x);
        return x;
    }
    tree[y].l = merge(x, tree[y].l);
    push_up(y);
    return y;
}

inline void insert(int key)
{
    split(root, key, x, y);
    root = merge(merge(x, new_node(key)), y);
}

inline void remove(int key)
{
    split(root, key, x, z);
    split(x, key - 1, x, y);
    y = merge(tree[y].l, tree[y].r);
    root = merge(merge(x, y), z);
}

inline int get_rank(int key)
{
    split(root, key - 1, x, y);
    int res = tree[x].siz + 1;
    root = merge(x, y);
    return res;
}

inline int get_key(int cur, int rank)
{
    if (rank == tree[tree[cur].l].siz + 1) {
        return tree[cur].key;
    }
    if (rank <= tree[tree[cur].l].siz) {
        return get_key(tree[cur].l, rank);
    }
    return get_key(tree[cur].r, rank - tree[tree[cur].l].siz - 1);
}

inline int get_prev(int key)
{
    split(root, key - 1, x, y);
    int cur = x;
    while (tree[cur].r) {
        cur = tree[cur].r;
    }
    int res = tree[cur].key;
    root = merge(x, y);
    return res;
}

inline int get_next(int key)
{
    split(root, key, x, y);
    int cur = y;
    while (tree[cur].l) {
        cur = tree[cur].l;
    }
    int res = tree[cur].key;
    root = merge(x, y);
    return res;
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    int n = read();
    while (n--) {
        int op = read(), x = read();
        if (op == 1) {
            insert(x);
        } else if (op == 2) {
            remove(x);
        } else if (op == 3) {
            write(get_rank(x), true);
        } else if (op == 4) {
            write(get_key(root, x), true);
        } else if (op == 5) {
            write(get_prev(x), true);
        } else {
            write(get_next(x), true);
        }
    }
    return 0;
}
```

[洛谷 P3391 【模板】文艺平衡树](https://www.luogu.com.cn/problem/P3391)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const double pi(acos(-1));
const int inf(0x3f3f3f3f);
const int maxn(1e5 + 10);
int idx, root;

struct node {
    int l, r;
    int key, val;
    int siz;
    bool rev;
} tree[maxn];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, char c)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (c) putchar(c);
}

inline int new_node(int key)
{
    ++idx;
    tree[idx].key = key;
    tree[idx].val = rand();
    tree[idx].siz = 1;
    return idx;
}

inline void push_up(int cur)
{
    tree[cur].siz = tree[tree[cur].l].siz + tree[tree[cur].r].siz + 1;
}

inline void push_down(int cur)
{
    if (tree[cur].rev) {
        swap(tree[cur].l, tree[cur].r);
        tree[tree[cur].l].rev ^= 1;
        tree[tree[cur].r].rev ^= 1;
        tree[cur].rev = false;
    }
}

inline void split(int cur, int siz, int& x, int& y)
{
    if (not cur) {
        x = y = 0;
    } else {
        push_down(cur);
        if (siz > tree[tree[cur].l].siz) {
            x = cur;
            split(tree[cur].r, siz - tree[tree[cur].l].siz - 1, tree[cur].r, y);
        } else {
            y = cur;
            split(tree[cur].l, siz, x, tree[cur].l);
        }
        push_up(cur);
    }
}

inline int merge(int x, int y)
{
    if (not x or not y) {
        return x + y;
    }
    if (tree[x].val < tree[y].val) {
        push_down(x);
        tree[x].r = merge(tree[x].r, y);
        push_up(x);
        return x;
    }
    push_down(y);
    tree[y].l = merge(x, tree[y].l);
    push_up(y);
    return y;
}

inline void reverse(int l, int r)
{
    int x, y, z;
    split(root, l - 1, x, y);
    split(y, r - l + 1, y, z);
    tree[y].rev ^= 1;
    root = merge(merge(x, y), z);
}

void dfs(int cur)
{
    if (not cur) return;
    push_down(cur);
    dfs(tree[cur].l);
    write(tree[cur].key, ' ');
    dfs(tree[cur].r);
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    int n = read(), m = read();
    for (int i = 1; i <= n; ++i) {
        root = merge(root, new_node(i));
    }
    while (m--) {
        int l = read(), r = read();
        reverse(l, r);
    }
    dfs(root);
    return 0;
}
```

#### 伸展树（Splay）

[洛谷 P3369 【模板】普通平衡树](https://www.luogu.com.cn/problem/P3369)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(1e5 + 10);
int idx, root;

struct node {
    int val;
    int siz, cnt;
    int fa, ch[2];
} tree[maxn];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline int new_node(int val)
{
    ++idx;
    tree[idx].val = val;
    tree[idx].siz = tree[idx].cnt = 1;
    return idx;
}

inline bool get_rel(int cur, int fa)
{
    return tree[fa].ch[1] == cur;
}

inline void connect(int cur, int fa, int rel)
{
    tree[fa].ch[rel] = cur;
    tree[cur].fa = fa;
}

inline void push_up(int cur)
{
    tree[cur].siz = tree[tree[cur].ch[0]].siz + tree[tree[cur].ch[1]].siz + tree[cur].cnt;
}

inline void rotate(int cur)
{
    int fa = tree[cur].fa;
    int gf = tree[fa].fa;
    bool rel = get_rel(cur, fa);
    connect(tree[cur].ch[rel ^ 1], fa, rel);
    connect(cur, gf, get_rel(fa, gf));
    connect(fa, cur, rel ^ 1);
    push_up(fa);
    push_up(cur);
}

inline void splaying(int cur, int top)
{
    while (tree[cur].fa not_eq top) {
        int fa = tree[cur].fa;
        int gf = tree[fa].fa;
        if (gf not_eq top) {
            get_rel(cur, fa) ^ get_rel(fa, gf) ? rotate(cur) : rotate(fa);
        }
        rotate(cur);
    }
    if (not top) {
        root = cur;
    }
}

inline void insert(int val)
{
    int cur = root, fa = 0;
    while (cur and tree[cur].val not_eq val) {
        fa = cur;
        cur = tree[cur].ch[val > tree[cur].val];
    }
    if (cur) {
        ++tree[cur].cnt;
        ++tree[cur].siz;
    } else {
        cur = new_node(val);
        connect(cur, fa, val > tree[fa].val);
    }
    splaying(cur, 0);
}

inline void remove(int val)
{
    int cur = root, fa = 0;
    while (tree[cur].val not_eq val) {
        fa = cur;
        cur = tree[cur].ch[val > tree[cur].val];
    }
    splaying(cur, 0);
    if (tree[cur].cnt > 1) {
        --tree[cur].cnt;
        --tree[cur].siz;
    } else if (tree[cur].ch[1]) {
        int nxt = tree[cur].ch[1];
        while (tree[nxt].ch[0]) {
            nxt = tree[nxt].ch[0];
        }
        splaying(nxt, cur);
        connect(tree[cur].ch[0], nxt, 0);
        root = nxt;
        tree[root].fa = 0;
        push_up(root);
    } else {
        root = tree[cur].ch[0];
        tree[root].fa = 0;
    }
}

inline int get_rank(int val)
{
    int cur = root, rank = 1;
    while (cur) {
        if (tree[cur].val == val) {
            rank += tree[tree[cur].ch[0]].siz;
            splaying(cur, 0);
            break;
        } else if (val < tree[cur].val) {
            cur = tree[cur].ch[0];
        } else {
            rank += tree[tree[cur].ch[0]].siz + tree[cur].cnt;
            cur = tree[cur].ch[1];
        }
    }
    return rank;
}

inline int get_val(int rank)
{
    int cur = root;
    while (cur) {
        int size_l = tree[tree[cur].ch[0]].siz;
        if (size_l + 1 <= rank and rank <= size_l + tree[cur].cnt) {
            splaying(cur, 0);
            break;
        } else if (size_l >= rank) {
            cur = tree[cur].ch[0];
        } else {
            rank -= size_l + tree[cur].cnt;
            cur = tree[cur].ch[1];
        }
    }
    return tree[cur].val;
}

inline int get_prev(int val)
{
    return get_val(get_rank(val) - 1);
}

inline int get_next(int val)
{
    return get_val(get_rank(val + 1));
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    int n = read();
    while (n--) {
        int op = read(), x = read();
        if (op == 1) {
            insert(x);
        } else if (op == 2) {
            remove(x);
        } else if (op == 3) {
            write(get_rank(x), true);
        } else if (op == 4) {
            write(get_val(x), true);
        } else if (op == 5) {
            write(get_prev(x), true);
        } else {
            write(get_next(x), true);
        }
    }
    return 0;
}
```

[洛谷 P3391 【模板】文艺平衡树](https://www.luogu.com.cn/problem/P3391)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(1e5 + 10);
int idx, root;

struct node {
    int val, siz;
    int fa, ch[2];
    bool rev;
} tree[maxn];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, char c)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (c) putchar(c);
}

inline int new_node(int val)
{
    ++idx;
    tree[idx].val = val;
    tree[idx].siz = 1;
    return idx;
}

inline void push_up(int cur)
{
    tree[cur].siz = tree[tree[cur].ch[0]].siz + tree[tree[cur].ch[1]].siz + 1;
}

inline void push_down(int cur)
{
    if (tree[cur].rev) {
        swap(tree[cur].ch[0], tree[cur].ch[1]);
        tree[tree[cur].ch[0]].rev ^= 1;
        tree[tree[cur].ch[1]].rev ^= 1;
        tree[cur].rev = false;
    }
}

inline int get_rel(int cur, int fa)
{
    return tree[fa].ch[1] == cur;
}

inline void connect(int cur, int fa, bool rel)
{
    tree[fa].ch[rel] = cur;
    tree[cur].fa = fa;
}

inline void rotate(int cur)
{
    int fa = tree[cur].fa;
    int gf = tree[fa].fa;
    bool rel = get_rel(cur, fa);
    connect(tree[cur].ch[rel ^ 1], fa, rel);
    connect(cur, gf, get_rel(fa, gf));
    connect(fa, cur, rel ^ 1);
    push_up(fa);
    push_up(cur);
}

inline void splaying(int cur, int top)
{
    while (tree[cur].fa not_eq top) {
        int fa = tree[cur].fa;
        int gf = tree[fa].fa;
        if (gf not_eq top) {
            get_rel(cur, fa) ^ get_rel(fa, gf) ? rotate(cur) : rotate(fa);
        }
        rotate(cur);
    }
    if (not top) {
        root = cur;
    }
}

inline void insert(int val)
{
    int cur = root, fa = 0;
    while (cur) {
        fa = cur;
        cur = tree[cur].ch[val > tree[cur].val];
    }
    cur = new_node(val);
    connect(cur, fa, val > tree[fa].val);
    splaying(cur, 0);
}

inline int get_id(int pos)
{
    int cur = root;
    while (cur) {
        push_down(cur);
        if (tree[tree[cur].ch[0]].siz >= pos) {
            cur = tree[cur].ch[0];
        } else if (tree[tree[cur].ch[0]].siz + 1 == pos) {
            return cur;
        } else {
            pos -= tree[tree[cur].ch[0]].siz + 1;
            cur = tree[cur].ch[1];
        }
    }
    return cur;
}

inline void reverse(int l, int r)
{
    int x = get_id(l - 1);
    int y = get_id(r + 1);
    splaying(x, 0);
    splaying(y, x);
    tree[tree[y].ch[0]].rev ^= 1;
}

void dfs(int cur, int n)
{
    if (not cur) return;
    push_down(cur);
    dfs(tree[cur].ch[0], n);
    if (tree[cur].val >= 1 and tree[cur].val <= n) {
        write(tree[cur].val, ' ');
    }
    dfs(tree[cur].ch[1], n);
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    int n = read(), m = read();
    for (int i = 0; i <= n + 1; ++i) {
        insert(i);
    }
    while (m--) {
        int l = read() + 1, r = read() + 1;
        reverse(l, r);
    }
    dfs(root, n);
    return 0;
}
```

#### 树套树

[洛谷 P3380 【模板】二逼平衡树（树套树）](https://www.luogu.com.cn/problem/P3380)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int inf(INT_MAX);
const int maxn(2e5 + 10);
const int maxm(1e7 + 10);
int idx, w[maxn];

struct splayNode {
    int val;
    int cnt, siz;
    int fa, ch[2];
} splay[maxm];

struct segNode {
    int l, r, root;
} segTree[maxn];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' or ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' and ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline int new_node(int val)
{
    ++idx;
    splay[idx].val = val;
    splay[idx].cnt = splay[idx].siz = 1;
    return idx;
}

inline bool get_rel(int cur, int fa)
{
    return splay[fa].ch[1] == cur;
}

inline void connect(int cur, int fa, bool rel)
{
    splay[fa].ch[rel] = cur;
    splay[cur].fa = fa;
}

inline void push_up(int cur)
{
    splay[cur].siz = splay[splay[cur].ch[0]].siz + splay[splay[cur].ch[1]].siz + splay[cur].cnt;
}

inline void rotate(int cur)
{
    int fa = splay[cur].fa;
    int gf = splay[fa].fa;
    bool rel = get_rel(cur, fa);
    connect(splay[cur].ch[rel xor 1], fa, rel);
    connect(cur, gf, get_rel(fa, gf));
    connect(fa, cur, rel xor 1);
    push_up(fa);
    push_up(cur);
}

inline void splaying(int cur, int top, int& rt)
{
    while (splay[cur].fa not_eq top) {
        int fa = splay[cur].fa;
        int gf = splay[fa].fa;
        if (gf not_eq top) {
            get_rel(cur, fa) xor get_rel(fa, gf) ? rotate(cur) : rotate(fa);
        }
        rotate(cur);
    }
    if (not top) rt = cur;
}

inline void insert(int val, int& rt)
{
    int cur = rt, fa = 0;
    while (cur and splay[cur].val not_eq val) {
        fa = cur;
        cur = splay[cur].ch[val > splay[cur].val];
    }
    if (cur) {
        ++splay[cur].cnt;
        ++splay[cur].siz;
    } else {
        cur = new_node(val);
        connect(cur, fa, val > splay[fa].val);
    }
    splaying(cur, 0, rt);
}

inline void remove(int val, int& rt)
{
    int cur = rt;
    while (cur and splay[cur].val not_eq val) {
        cur = splay[cur].ch[val > splay[cur].val];
    }
    splaying(cur, 0, rt);
    if (splay[cur].cnt > 1) {
        --splay[cur].cnt;
        --splay[cur].siz;
    } else if (splay[cur].ch[1]) {
        int nxt = splay[cur].ch[1];
        while (splay[nxt].ch[0]) {
            nxt = splay[nxt].ch[0];
        }
        splaying(nxt, cur, rt);
        connect(splay[cur].ch[0], nxt, false);
        rt = nxt;
        splay[rt].fa = splay[cur].ch[1] = 0;
        push_up(rt);
    } else {
        rt = splay[cur].ch[0];
        splay[rt].fa = splay[cur].ch[0] = 0;
        push_up(rt);
    }
}

inline int get_rank(int val, int& rt)
{
    int cur = rt, rank = 0;
    while (cur) {
        if (val < splay[cur].val) {
            cur = splay[cur].ch[0];
        } else if (val > splay[cur].val) {
            rank += splay[splay[cur].ch[0]].siz + splay[cur].cnt;
            cur = splay[cur].ch[1];
        } else {
            rank += splay[splay[cur].ch[0]].siz;
            splaying(cur, 0, rt);
            break;
        }
    }
    return rank;
}

inline int get_val(int rank, int& rt)
{
    int cur = rt;
    while (cur) {
        if (splay[splay[cur].ch[0]].siz >= rank) {
            cur = splay[cur].ch[0];
        } else if (splay[splay[cur].ch[0]].siz + splay[cur].cnt >= rank) {
            splaying(cur, 0, rt);
            break;
        } else {
            rank -= splay[splay[cur].ch[0]].siz + splay[cur].cnt;
            cur = splay[cur].ch[1];
        }
    }
    return splay[cur].val;
}

inline int get_prev(int val, int& rt)
{
    return get_val(get_rank(val, rt), rt);
}

inline int get_next(int val, int& rt)
{
    return get_val(get_rank(val + 1, rt) + 1, rt);
}

inline int ls(int cur)
{
    return cur << 1;
}

inline int rs(int cur)
{
    return cur << 1 bitor 1;
}

void build(int cur, int l, int r)
{
    segTree[cur].l = l;
    segTree[cur].r = r;
    segTree[cur].root = new_node(-inf);
    insert(inf, segTree[cur].root);
    for (int i = l; i <= r; ++i) {
        insert(w[i], segTree[cur].root);
    }
    if (l == r) return;
    int mid = (l + r) >> 1;
    build(ls(cur), l, mid);
    build(rs(cur), mid + 1, r);
}

inline int query_rank(int cur, int l, int r, int v)
{
    if (segTree[cur].l == l and segTree[cur].r == r) {
        return get_rank(v, segTree[cur].root) - 1;
    }
    int mid = (segTree[cur].l + segTree[cur].r) >> 1;
    if (r <= mid) {
        return query_rank(ls(cur), l, r, v);
    }
    if (l > mid) {
        return query_rank(rs(cur), l, r, v);
    }
    return query_rank(ls(cur), l, mid, v) + query_rank(rs(cur), mid + 1, r, v);
}

inline int query_val(int l, int r, int k)
{
    int low = 0, high = 1e8;
    while (low < high) {
        int mid = (low + high + 1) >> 1;
        if (query_rank(1, l, r, mid) + 1 <= k) {
            low = mid;
        } else {
            high = mid - 1;
        }
    }
    return high;
}

inline void update(int cur, int p, int v)
{
    remove(w[p], segTree[cur].root);
    insert(v, segTree[cur].root);
    if (segTree[cur].l == segTree[cur].r) return;
    int mid = (segTree[cur].l + segTree[cur].r) >> 1;
    if (p <= mid) {
        update(ls(cur), p, v);
    } else {
        update(rs(cur), p, v);
    }
}

inline int query_prev(int cur, int l, int r, int v)
{
    if (segTree[cur].l == l and segTree[cur].r == r) {
        return get_prev(v, segTree[cur].root);
    }
    int mid = (segTree[cur].l + segTree[cur].r) >> 1;
    if (r <= mid) {
        return query_prev(ls(cur), l, r, v);
    }
    if (l > mid) {
        return query_prev(rs(cur), l, r, v);
    }
    return max(query_prev(ls(cur), l, mid, v), query_prev(rs(cur), mid + 1, r, v));
}

inline int query_next(int cur, int l, int r, int v)
{
    if (segTree[cur].l == l and segTree[cur].r == r) {
        return get_next(v, segTree[cur].root);
    }
    int mid = (segTree[cur].l + segTree[cur].r) >> 1;
    if (r <= mid) {
        return query_next(ls(cur), l, r, v);
    }
    if (l > mid) {
        return query_next(rs(cur), l, r, v);
    }
    return min(query_next(ls(cur), l, mid, v), query_next(rs(cur), mid + 1, r, v));
}

int main()
{
#ifdef ONLINE_JUDGE
#else
    freopen("input.txt", "r", stdin);
#endif
    int n = read(), m = read();
    for (int i = 1; i <= n; ++i) {
        w[i] = read();
    }
    build(1, 1, n);
    while (m--) {
        int op = read();
        if (op == 1) {
            int l = read(), r = read(), k = read();
            write(query_rank(1, l, r, k) + 1, true);
        } else if (op == 2) {
            int l = read(), r = read(), k = read();
            write(query_val(l, r, k), true);
        } else if (op == 3) {
            int p = read(), k = read();
            update(1, p, k);
            w[p] = k;
        } else if (op == 4) {
            int l = read(), r = read(), k = read();
            write(query_prev(1, l, r, k), true);
        } else {
            int l = read(), r = read(), k = read();
            write(query_next(1, l, r, k), true);
        }
    }
    return 0;
}
```

#### 树链剖分

[洛谷 P3384 【模板】轻重链剖分](https://www.luogu.com.cn/problem/P3384)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(1e5 + 10);
const int maxm(2e5 + 10);
int mod, ecnt, v[maxn], head[maxn];
int dep[maxn], siz[maxn], fa[maxn], son[maxn];
int tim, dfn[maxn], top[maxn], w[maxn];

struct edge {
    int to, nxt;
} edges[maxm];

struct node {
    int l, r;
    int sum, lz;
} tree[maxn << 2];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' || ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' && ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline void addEdge(int u, int v)
{
    edges[ecnt].to = v;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

inline int ls(int cur)
{
    return cur << 1;
}

inline int rs(int cur)
{
    return cur << 1 bitor 1;
}

inline void push_up(int cur)
{
    tree[cur].sum = tree[ls(cur)].sum + tree[rs(cur)].sum;
}

inline void push_down(int cur)
{
    if (tree[cur].lz) {
        tree[ls(cur)].lz = (tree[ls(cur)].lz + tree[cur].lz) % mod;
        tree[rs(cur)].lz = (tree[rs(cur)].lz + tree[cur].lz) % mod;
        tree[ls(cur)].sum = (tree[ls(cur)].sum + (tree[ls(cur)].r - tree[ls(cur)].l + 1) * tree[cur].lz) % mod;
        tree[rs(cur)].sum = (tree[rs(cur)].sum + (tree[rs(cur)].r - tree[rs(cur)].l + 1) * tree[cur].lz) % mod;
        tree[cur].lz = false;
    }
}

void build(int cur, int l, int r)
{
    tree[cur].l = l;
    tree[cur].r = r;
    if (l == r) {
        tree[cur].sum = w[l];
        return;
    }
    int mid = (l + r) >> 1;
    build(ls(cur), l, mid);
    build(rs(cur), mid + 1, r);
    push_up(cur);
}

void update(int cur, int l, int r, int v)
{
    if (tree[cur].l == l and tree[cur].r == r) {
        tree[cur].sum = (tree[cur].sum + (r - l + 1) * v) % mod;
        tree[cur].lz = (tree[cur].lz + v) % mod;
        return;
    }
    push_down(cur);
    int mid = (tree[cur].l + tree[cur].r) >> 1;
    if (r <= mid) {
        update(ls(cur), l, r, v);
    } else if (l > mid) {
        update(rs(cur), l, r, v);
    } else {
        update(ls(cur), l, mid, v);
        update(rs(cur), mid + 1, r, v);
    }
    push_up(cur);
}

int query(int cur, int l, int r)
{
    if (tree[cur].l == l and tree[cur].r == r) {
        return tree[cur].sum;
    }
    push_down(cur);
    int mid = (tree[cur].l + tree[cur].r) >> 1;
    if (r <= mid) {
        return query(ls(cur), l, r);
    }
    if (l > mid) {
        return query(rs(cur), l, r);
    }
    return (query(ls(cur), l, mid) + query(rs(cur), mid + 1, r)) % mod;
}

void dfs1(int cur, int pre)
{
    fa[cur] = pre;
    dep[cur] = dep[pre] + 1;
    siz[cur] = 1;
    int maxsize = -1;
    for (int i = head[cur]; compl i; i = edges[i].nxt) {
        int nxt = edges[i].to;
        if (nxt == pre) continue;
        dfs1(nxt, cur);
        siz[cur] += siz[nxt];
        if (siz[nxt] > maxsize) {
            maxsize = siz[nxt];
            son[cur] = nxt;
        }
    }
}

void dfs2(int cur, int tp)
{
    dfn[cur] = ++tim;
    w[tim] = v[cur];
    top[cur] = tp;
    if (not son[cur]) return;
    dfs2(son[cur], tp);
    for (int i = head[cur]; compl i; i = edges[i].nxt) {
        int nxt = edges[i].to;
        if (nxt not_eq fa[cur] and nxt not_eq son[cur]) {
            dfs2(nxt, nxt);
        }
    }
}

void update_chain(int u, int v, int val)
{
    val %= mod;
    while (top[u] not_eq top[v]) {
        if (dep[top[u]] < dep[top[v]]) {
            swap(u, v);
        }
        update(1, dfn[top[u]], dfn[u], val);
        u = fa[top[u]];
    }
    if (dep[u] > dep[v]) {
        swap(u, v);
    }
    update(1, dfn[u], dfn[v], val);
}

int query_chain(int u, int v)
{
    int res = 0;
    while (top[u] not_eq top[v]) {
        if (dep[top[u]] < dep[top[v]]) {
            swap(u, v);
        }
        res += query(1, dfn[top[u]], dfn[u]);
        u = fa[top[u]];
    }
    if (dep[u] > dep[v]) {
        swap(u, v);
    }
    res += query(1, dfn[u], dfn[v]);
    return res % mod;
}

inline void update_son(int cur, int val)
{
    update(1, dfn[cur], dfn[cur] + siz[cur] - 1, val);
}

inline int query_son(int cur)
{
    return query(1, dfn[cur], dfn[cur] + siz[cur] - 1);
}

int main()
{
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
#endif
    memset(head, -1, sizeof head);
    int n = read(), m = read(), r = read();
    mod = read();
    for (int i = 1; i <= n; ++i) {
        v[i] = read();
    }
    for (int i = 1; i <= n - 1; ++i) {
        int u = read(), v = read();
        addEdge(u, v);
        addEdge(v, u);
    }
    dfs1(r, r);
    dfs2(r, r);
    build(1, 1, n);
    while (m--) {
        int op = read();
        if (op == 1) {
            int x = read(), y = read(), z = read();
            update_chain(x, y, z);
        } else if (op == 2) {
            int x = read(), y = read();
            write(query_chain(x, y), true);
        } else if (op == 3) {
            int x = read(), z = read();
            update_son(x, z);
        } else {
            int x = read();
            write(query_son(x), true);
        }
    }
    return 0;
}
```

#### 点分治

[洛谷 P3806 【模板】点分治1](https://www.luogu.com.cn/problem/P3806)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(1e4 + 10);
const int maxm(1e2 + 10);
const int maxk(2e7 + 10);
int ecnt, head[maxn];
int que[maxm], ans[maxn];
int rt, tot, root[maxn], siz[maxn], maxp[maxn];
int cnt, tmp[maxn], dis[maxn];
bool vis[maxn], judge[maxk];

struct edge {
    int to, wt, nxt;
} edges[maxn << 1];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' or ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' and ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline void addEdge(int u, int v, int w)
{
    edges[ecnt].to = v;
    edges[ecnt].wt = w;
    edges[ecnt].nxt = head[u];
    head[u] = ecnt++;
}

void getRoot(int u, int f)
{
    siz[u] = 1;
    maxp[u] = 0;
    for (int i = head[u]; compl i; i = edges[i].nxt) {
        int v = edges[i].to;
        if (vis[v] or v == f) {
            continue;
        }
        getRoot(v, u);
        siz[u] += siz[v];
        maxp[u] = max(maxp[u], siz[v]);
    }
    maxp[u] = max(maxp[u], tot - siz[u]);
    if (maxp[u] < maxp[rt]) {
        rt = u;
    }
}

void getDis(int u, int f)
{
    tmp[cnt++] = dis[u];
    for (int i = head[u]; compl i; i = edges[i].nxt) {
        int v = edges[i].to, w = edges[i].wt;
        if (vis[v] or v == f) {
            continue;
        }
        dis[v] = dis[u] + w;
        getDis(v, u);
    }
}

void solve(int u, int m)
{
    static queue<int> q;
    for (int i = head[u]; compl i; i = edges[i].nxt) {
        int v = edges[i].to, w = edges[i].wt;
        if (vis[v]) {
            continue;
        }
        cnt = 0;
        dis[v] = w;
        getDis(v, u);
        for (int j = 0; j < cnt; ++j) {
            for (int k = 0; k < m; ++k) {
                if (que[k] >= tmp[j]) {
                    ans[k] or_eq judge[que[k] - tmp[j]];
                }
            }
        }
        for (int j = 0; j < cnt; ++j) {
            q.push(tmp[j]);
            judge[tmp[j]] = true;
        }
    }
    while (not q.empty()) {
        judge[q.front()] = false;
        q.pop();
    }
}

void divide(int u, int m)
{
    vis[u] = judge[0] = true;
    solve(u, m);
    for (int i = head[u]; compl i; i = edges[i].nxt) {
        int v = edges[i].to;
        if (vis[v]) {
            continue;
        }
        maxp[rt = 0] = tot = siz[v];
        getRoot(v, 0);
        getRoot(rt, 0);
        divide(rt, m);
    }
}

int main()
{
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
#endif
    memset(head, -1, sizeof head);
    int n = read(), m = read();
    for (int i = 0; i < n - 1; ++i) {
        int u = read(), v = read(), w = read();
        addEdge(u, v, w);
        addEdge(v, u, w);
    }
    for (int i = 0; i < m; ++i) {
        que[i] = read();
    }
    tot = maxp[0] = n;
    getRoot(1, 0);
    getRoot(rt, 0);
    divide(rt, m);
    for (int i = 0; i < m; ++i) {
        puts(ans[i] ? "AYE" : "NAY");
    }
    return 0;
}
```

#### 动态树（Link-Cut Tree）

[洛谷 P3690 【模板】Link Cut Tree （动态树）](https://www.luogu.com.cn/problem/P3690)

```cpp
#include <bits/stdc++.h>

using namespace std;
using ll = long long;
using p = pair<int, int>;
const int maxn(1e5 + 10);

struct node {
    int l, r;
    int ch[2], fa;
    int val, sum;
    bool rev;
} tree[maxn];

template<typename T = int>
inline const T read()
{
    T x = 0, f = 1;
    char ch = getchar();
    while (ch < '0' or ch > '9') {
        if (ch == '-') f = -1;
        ch = getchar();
    }
    while (ch >= '0' and ch <= '9') {
        x = (x << 3) + (x << 1) + ch - '0';
        ch = getchar();
    }
    return x * f;
}

template<typename T>
inline void write(T x, bool ln)
{
    if (x < 0) {
        putchar('-');
        x = -x;
    }
    if (x > 9) write(x / 10, false);
    putchar(x % 10 + '0');
    if (ln) putchar(10);
}

inline int& ls(int cur)
{
    return tree[cur].ch[0];
}

inline int& rs(int cur)
{
    return tree[cur].ch[1];
}

inline bool get_rel(int cur, int fa)
{
    return tree[fa].ch[1] == cur;
}

inline void connect(int cur, int fa, bool rel)
{
    tree[fa].ch[rel] = cur;
    tree[cur].fa = fa;
}

inline bool is_root(int cur)
{
    return ls(tree[cur].fa) not_eq cur and rs(tree[cur].fa) not_eq cur;
}

inline void push_up(int cur)
{
    tree[cur].sum = tree[cur].val xor tree[ls(cur)].sum xor tree[rs(cur)].sum;
}

inline void reverse(int cur)
{
    swap(ls(cur), rs(cur));
    tree[cur].rev xor_eq 1;
}

inline void push_down(int cur)
{
    if (tree[cur].rev) {
        reverse(ls(cur));
        reverse(rs(cur));
        tree[cur].rev = false;
    }
}

inline void rotate(int cur)
{
    int fa = tree[cur].fa;
    int gf = tree[fa].fa;
    bool rel = get_rel(cur, fa);
    connect(tree[cur].ch[rel xor 1], fa, rel);
    tree[cur].fa = gf;
    if (not is_root(fa)) {
        tree[gf].ch[get_rel(fa, gf)] = cur;
    }
    connect(fa, cur, rel xor 1);
    push_up(fa);
    push_up(cur);
}

inline void push_all(int cur)
{
    if (not is_root(cur)) {
        push_all(tree[cur].fa);
    }
    push_down(cur);
}

inline void splaying(int cur)
{
    push_all(cur);
    while (not is_root(cur)) {
        int fa = tree[cur].fa;
        int gf = tree[fa].fa;
        if (not is_root(fa)) {
            get_rel(cur, fa) xor get_rel(fa, gf) ? rotate(cur) : rotate(fa);
        }
        rotate(cur);
    }
}

inline void access(int cur)
{
    for (int pre = 0; cur; cur = tree[cur].fa) {
        splaying(cur);
        rs(cur) = pre;
        push_up(cur);
        pre = cur;
    }
}

inline void make_root(int cur)
{
    access(cur);
    splaying(cur);
    reverse(cur);
}

inline int find_root(int cur)
{
    access(cur);
    splaying(cur);
    while (ls(cur)) {
        push_down(cur);
        cur = ls(cur);
    }
    splaying(cur);
    return cur;
}

inline void link(int u, int v)
{
    make_root(u);
    if (find_root(v) not_eq u) {
        tree[u].fa = v;
    }
}

inline void cut(int u, int v)
{
    make_root(u);
    if (find_root(v) == u and tree[v].fa == u and not ls(v)) {
        rs(u) = tree[v].fa = 0;
        push_up(u);
    }
}

inline void split(int u, int v)
{
    make_root(u);
    access(v);
    splaying(v);
}

int main()
{
#ifndef ONLINE_JUDGE
    freopen("input.txt", "r", stdin);
#endif
    int n = read(), m = read();
    for (int i = 1; i <= n; ++i) {
        tree[i].val = read();
    }
    while (m--) {
        int op = read(), x = read(), y = read();
        if (op == 0) {
            split(x, y);
            write(tree[y].sum, true);
        } else if (op == 1) {
            link(x, y);
        } else if (op == 2) {
            cut(x, y);
        } else {
            splaying(x);
            tree[x].val = y;
            push_up(x);
        }
    }
    return 0;
}
```