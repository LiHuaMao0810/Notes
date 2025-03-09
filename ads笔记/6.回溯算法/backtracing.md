# 回溯算法
感觉更像dfs深搜
按顺序选择某个选项，进行下一层搜索；
如果不能解决，撤回刚刚的操作，选择其他选项，进行下一层搜索。

>
    bool Backtracking ( int i )
    {   Found = false;
        if ( i > N )
            return true; /* solved with (x1, …, xN) */
        for ( each xi in Si ) { 
            /* check if satisfies the restriction R */
            OK = Check((x1, …, xi) , R ); /* pruning */
            if ( OK ) {
                Count xi in;
                Found = Backtracking( i+1 );
                if ( !Found )
                    Undo( i ); /* recover to (x1, …, xi-1) */
            }
            if ( Found ) break; 
        }
        return Found;
    }

## 案例
### 八皇后
### 收费站重建问题(The Turnpike Reconstruction Prblem)

### α-β剪枝 
<https://oi-wiki.org/search/alpha-beta/>
