# $KMP$

## 求$nxt$数组

```c++
vector<int> nxt(m + 10); nxt[0] = -1;
auto pre = [&]() -> void {
    int j = 0, k = -1;
    while(j < m){
        if(k == -1 || t[j] == t[k]) nxt[++ j] = ++ k;
        else k = nxt[k];
    }
    return ;
};
pre();
```

## $kmp$匹配

```c++
auto kmp = [&]() -> void {
    int j = 0, k = 0;
    while(j < n){
        if(k == -1 || s[j] == t[k]) j ++, k ++;
        else k = nxt[k];
        if(k == m){
            cout << j - k + 1 << endl;
            k = nxt[k];
        }
    }
    return ;
};
kmp();
```

