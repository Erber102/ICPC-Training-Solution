# B. Like the Bitset
## 基本信息
- **比赛**：cf round 1046 div2
- **题目状态**：赛时做出
- **补题日期**：2025-8-29
- **题目链接**：https://codeforces.com/contest/2136/problem/B
- **难度**：easy
- **标签**：[[贪心]] [[滑动窗口]]
- **相似题**：{{题号或纯文本链接（如有）}}

---

## 题意简述
给定一个长度为$n$的二进制字符串$s$和一个正整数$k$。我们希望构造一个长度为$n$的排列，使得对于任何一个长度大于$k$的区间$[l,r]$，如果将区间内最大值记为$MAX$，满足 $\forall i \in [l,r], s_{i} = 1, p_{i}\ne MAX$。
$1≤𝑛≤2⋅10^5, 1≤𝑘≤𝑛$

---

## 解题过程
一个比较显然的构造是让$s_i = 1$的位置$p_i$尽可能小，$s_i = 0$的位置$p_i$尽可能大。然后我们再来证明这种方法的合理性。
先考虑排列不存在的情况。如果$s$中有连续$k$个1，那么其中一定有一个最大值。如果不存在这样连续$k$个1，那么每连续$k$个数中一定会有0，那么只要让$s_i=0$时最小的$p_i$大于$s_i=1$时最大的$p_i$就行。


```cpp
#include<iostream>
#include<vector>

using namespace std;

void solve() {
    int n, k;
    string s;
    cin >> n >> k;
    cin >> s;
    vector<int> result(n);
    int cnt = 0;
    for (int i = 0; i < k; i++) cnt += (s[i] == '1');
    if (cnt == k) {
        cout << "NO" << '\n';
        return;
    }
    for (int i = k; i < n; i++) {
        int head = i - k;
        cnt += (s[i] == '1') - (s[head] == '1');
        if (cnt == k) {
            cout << "NO" << '\n';
            return;
        }
    }
    int ans = 1;
    for (int i = 0; i < n; i++) {
        if (s[i] == '1') result[i] = ans++;
    }
    for (int i = 0; i < n; i++) {
        if (s[i] == '0') result[i] = ans++;
    }
    cout << "YES" << '\n';
    for (int i = 0; i < n; i++) {
        cout << result[i] << " \n"[i == n - 1];
    }
}


int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    int TC;
    cin >> TC;
    while (TC--) solve();
    return 0;
}
```
