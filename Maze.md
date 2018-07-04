# 迷宫问题求解<br>
  * 求解思路<br>
  对一个迷宫矩阵对每个位置按照顺时针方向进行递归判断，找到通路
  非递归方法用回溯的思想，运用栈将点导入导出进行判断<br>
  * 求解代码<br>
```
#pragma once
#include<iostream>
using namespace std;

//#define _CRT_SECURE_NO_WARNINGS
#pragma warning(disable:4996)

#include<assert.h>
#include<stdio.h>
#include<stack>



struct Pos
{
    int _row;
    int _col;
};

template<size_t M,size_t N>
class Maze
{
public:
    Maze()
    {
        FILE* fout = fopen("MazeMap.txt", "r");
        assert(fout);

        for (size_t i = 0; i < M; ++i)
        {
            for (size_t j = 0; j < N;)
            {
                char ch = fgetc(fout);
                if (ch == '1' || ch == '0')
                //构建一个以文字内数字为元素的二维数组，1表示不可走，0表示可走
                {
                    _maze[i][j] = ch - '0';
                    ++j;
                }
            }
        }
    }

    bool CheckAccess(Pos pos)//检查下一个位置是否可以走
    {
        if ((pos._row<M)&&(pos._col<N)&&(_maze[pos._row][pos._col] == 0))
        {
            return true;
        }
        return false;
    }

    bool GetMazePathR(Pos cur)//递归走
    {
        _maze[cur._row][cur._col] = 2;//用2表示走过的位置来记录
        //找到出口
        if (cur._row == M - 1)
        {
            return true;
        }
        //探测
        Pos next = cur;
        //上
        next._row -= 1;
        if (CheckAccess(next))
        {
            if (GetMazePathR(next))
                return true;
        }
        //右
        next = cur;
        next._col += 1;
        if (CheckAccess(next))
        {
            if (GetMazePathR(next))
                return true;
        }
        //下
        next = cur;
        next._row += 1;
        if (CheckAccess(next))
        {
            if (GetMazePathR(next))
                return true;
        }
        //左
        next = cur;
        next._col -= 1;
        if (CheckAccess(next))
        {
            if (GetMazePathR(next))
                return true;
        }
        return false;
    }

    bool GetMazePathNonR(Pos entry)//非递归走，用回溯的方法
    {
        stack<Pos> path;
        path.push(entry);//先将入口push到栈中
        while (!path.empty())
        {
            Pos cur = path.top();
            _maze[cur._row][cur._col] = 2;//用2表示走过的位置来记录
            Pos next = cur;
            if (cur._row == M-1)
            {
                return true;
            }
            //向上探测
            --next._row;
            if (CheckAccess(next))
            {
                path.push(next);
                continue;
            }
            next = cur;//如果不能走，让next回到初始的cur的位置

            //向右探测
            ++next._col;
            if (CheckAccess(next))
            {
                path.push(next);
                continue;
            }
            next = cur;

            //向下探测
            ++next._row;
            if (CheckAccess(next))
            {
                path.push(next);
                continue;
            }
            next = cur;

            //向左探测
            --next._col;
            if (CheckAccess(next))
            {
                path.push(next);
                continue;
            }
            next = cur;

            _maze[cur._row][cur._col] = 3;
            //如果一个if条件都没有进，那么该位置是死胡同,不能走，用3来表示
            path.pop();//将该位置pop出栈
        }
        return false;
    }

    bool CheckAccess(Pos cur,Pos next)
    //与上一个CheckAccess函数形成重载
    {
        if ((next._row>=M) || (next._col>=N) )
        {
            return false;
        }
        if ((_maze[next._row][next._col] == 0)
         || (_maze[next._row][next._col]> (_maze[cur._row][cur._col] + 1)))
        {
            return true;
        }
        return false;
    }

    void GetMazeShortPathR(Pos cur, stack<Pos>& paths, stack<Pos>& shortpaths)
    //求解出口的最短路径
    {
        paths.push(cur);
        //找到出口
        if (cur._row == M - 1)
        {
            if (shortpaths.empty()||(shortpaths.size()>paths.size()))
            {
                shortpaths = paths;
            }
            return;
        }

        //探测
        Pos next = cur;

        //上
        next._row -= 1;
        if (CheckAccess(cur,next))
        {
            _maze[next._row][next._col] = _maze[cur._row][cur._col] + 1;
            GetMazeShortPathR(next, paths, shortpaths);
        }

        //右
        next = cur;
        next._col += 1;
        if (CheckAccess(cur, next))
        {
            _maze[next._row][next._col] = _maze[cur._row][cur._col] + 1;
            GetMazeShortPathR(next, paths, shortpaths);
        }

        //下
        next = cur;
        next._row += 1;
        if (CheckAccess(cur,next))
        {
            _maze[next._row][next._col] = _maze[cur._row][cur._col] + 1;
            GetMazeShortPathR(next, paths, shortpaths);
        }

        //左
        next = cur;
        next._col -= 1;
        if (CheckAccess(cur,next))
        {
            _maze[next._row][next._col] = _maze[cur._row][cur._col] + 1;
            GetMazeShortPathR(next, paths, shortpaths);
        }

        paths.pop();
    }

    void Print()
    {
        for (size_t i = 0; i < M; ++i)
        {
            for (size_t j = 0; j < N;++j)
            {
                cout << _maze[i][j] << " ";
            }
            cout << endl;
        }
        cout << endl;
    }
protected:
    int _maze[M][N];
};

```