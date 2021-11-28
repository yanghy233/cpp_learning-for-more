#### 在多进程的语境下，对volatile的语义分析 

```cpp
volatile int ans = 2;

int main(int argc, char *argv[]) {
    int pid;
    ans++;
    if((pid = fork()) == 0){
        ans+=2;
        printf("child's pid = %d, ans = %d\n", getpid(), ans);
    }
    else{
        ans++;
        printf("father's pid = %d, ans = %d\n", getpid(), ans);
    }

    printf("pid = %d, final ans = %d\n", getpid(), ans);
    return 0;
}


/* 结果：
father's pid = 4422, ans = 4
child's pid = 4423, ans = 5
pid = 4422, final ans = 4
pid = 4423, final ans = 5
```



volatile作用：每次直接从内存地址存取值进行修改，防止编译器的缓存优化

当创建了一个子进程后，子进程继承父进程的所有变量，但由于子进程存放变量地址与父进程不同，最后父进程的结果与子进程也不同；

即每个进程有独立的对全局变量的存取，可以看作分离成两个变量了

