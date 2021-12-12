---
title: "优化算法之Python穷举数独"
categories: [ "开源项目","算法解析" ]
tags: [  ]
draft: false
slug: "sudoku"
date: "2020-02-26 20:22:00"
---

# 前言
本文的 Github 仓库

> https://github.com/taurusxin/Sudoku

# Python 穷举 Sudoku
## 玩法简介（来自百度百科）
数独盘面是个九宫，每一宫又分为九个小格。在这八十一格中给出一定的已知数字和解题条件，利用逻辑和推理，在其他的空格上填入1-9的数字。使1-9每个数字在每一行、每一列和每一宫中都只出现一次，所以又称“九宫格”。

从初中开始第一次接触数独，大大小小的也玩过很多。
学了计算机之后自然就会用算法去解决一些问题，本文讲解的就是在穷举数独之时，对相应的算法进行优化。

## 开发环境

 1. VSCode
 2. Python 3.x

在开始之前，先配置一下 VSCode 的运行环境，可以直接克隆我的Github仓库。
因为默认VSCode在文件夹下调试的时候，会以当前正在编辑的文件传入命令行，如果正在编辑其它的例如json/xml等文件，那运行就出错了。
在项目路径下建立文件夹 `.vscode`，然后新建文件 `launch.json`来构建运行的脚本。

    {
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Sudoku",
                "type": "python",
                "request": "launch",
                "program": "${workspaceRoot}/Sudoku.py",
                "args": ["test1.csv"],
                "console": "integratedTerminal"
            }
        ]
    }

表示无论在哪个标签页，就会将 `Sudoku.py` 作为要运行的程序源文件传入解释器，并且将 `test1.csv` 作为第一个参数传入。

## 导入文件

    lines = None
    if len(argv) != 2:
        print(usage)
        return
    f = None
    if argv[1] in ('-h', '--help'):
        print(usage)
        return
    try:
        f = open(argv[1], 'r', encoding='utf-8')
    except IOError:
        print('Error: File do not exists!')
        return
    
    lines = f.read().splitlines()
    if len(lines) != 9:
        print('Error: Sudoku format error, type -h or --help for more.')
        return
    grid = map(lambda i: i.split(","), lines)
    
    def new_int(t):
        return int(t) if t else 0
    
    grid = [list(map(new_int, i)) for i in grid]

初始的几行是命令行的解析及文件的打开
`grid`定义了整一个表格的列表，每一行为一个列表，9行构成了整个二维列表
这里看到，我定义了一个`new_int`函数，这因为 Python 自带的 int()函数不能将非0的字符转为0。

## class 定义

    class Sudoku(object):
        def __init__(self, board):
            self.b = board
            self.t = 0

构造函数引入了1个参数`board`，即数独二维列表的表格。
`t`为尝试的次数

## 尝试前的准备

    def check_is_legal(self):
        # 判断每一行是否有效
        for i in range(9):
            for j in self.b[i]:
                if (j not in range(0, 10)) or (self.b[i].count(j) > 1 and j != 0):
                    return False, 'There are %d same number %d in line %d.' % (self.b[i].count(j), j, (i + 1))
        # 判断每一列是否有效
        columns = [[r[col] for r in self.b] for col in range(len(self.b[0]))]
        for i in range(9):
            for j in columns[i]:
                if (j not in range(0, 10)) or (self.b[i].count(j) > 1 and j != 0):
                    return False, 'There are %d same number %d in column %d.' % (self.b[i].count(j), j, (i + 1))
        # 判断九宫格是否有效
        for i in range(3):
            for j in range(3):
                grid = [tem[j * 3:(j + 1) * 3] for tem in self.b[i * 3:(i + 1) * 3]]
                merge_str = grid[0] + grid[1] + grid[2]  # 合并为一个list[]
                for m in merge_str:
                    if (m not in range(0, 10)) or (merge_str.count(m) > 1 and m != 0):
                        return False, 'There are %d same number %d in block %d.' \
                               % (merge_str.count(m), m, (3 * i + j + 1))
        return True, 'OK'

    def solve(self):
        is_legal, error_s = self.check_is_legal()
        if not is_legal:
            print("Error: The sudoku is illegal!")
            print(error_s)
            return

        begin = datetime.datetime.now()
        if self.b[0][0] == 0:
            self.try_it(0, 0)  # 如果第一格为空，则从此开始尝试
        else:
            x, y = self.get_next(0, 0)  # 否则就获得从第一个格子的下一个空格子开始
            self.try_it(x, y)

在开始尝试之前，当然要先检测一下数独是否合法，即给出的数独题目，是否每一行，每一列，每一宫都不得重复1-9

## 尝试函数的递归

    def try_it(self, x, y):  # 主循环
        if self.b[x][y] == 0:
            possible = self.possible_num(x, y)
            for i in possible:  # 从可能的数字中尝试
                self.t += 1
                self.b[x][y] = i  # 将可能的数字填入0格
                next_x, next_y = self.get_next(x, y)  # 得到下一个0格
                if next_x == -1:  # 如果无下一个0格
                    return True  # 返回True
                else:  # 如果有下一个0格，递归判断下一个0格直到填满数独
                    end = self.try_it(next_x, next_y)
                    if not end:  # 在递归过程中存在不符合条件的，即 使try_it函数返回None的项
                        self.b[x][y] = 0  # 回朔到上一层继续
                    else:
                        return True

在`solve`函数中，如果第一格格子为空，程序尝试着从第一格格子开始尝试，否则就从第一格格子的下一个开始尝试。
尝试的法则则为，用`possible_num`函数列举出当前格子可能的情况列表，然后从中逐个尝试，若遇到了这个格子为空，并且这个格子没有可能的数值，那么就发生错误，则回到上一个格子重新开始，将此格子清除

## 辅助功能函数

    def get_next(self, x, y):  # 得到下一个未填项
        for next_solution in range(y + 1, 9):
            if self.b[x][next_solution] == 0:
                return x, next_solution
        for row_n in range(x + 1, 9):
            for col_n in range(0, 9):
                if self.b[row_n][col_n] == 0:
                    return row_n, col_n
        return -1, -1  # 若无下一个未填项，返回-1

    block_in_grid = {(x, y): (x // 3) * 3 + (y // 3) for x in range(9) for y in range(9)}
    grid_get_block = {x: [] for x in range(9)}

    for i, j in block_in_grid.items():
        grid_get_block[j].append(i)

    def possible_num(self, x, y):  # 获得一个格子可能的数字集合
        whole = set(range(1, 10))
        x_set = set(self.b[x])
        y_set = {self.b[i][y] for i in range(9)}
        block_num = self.block_in_grid[(x, y)]
        block_set = {self.b[i][j] for i, j in self.grid_get_block[block_num]}
        return whole - x_set - y_set - block_set

这两个函数应该比较好理解，获得下一个未填格子，就是在表格中从当给出格子开始遍历，找到一个为非1-9的格子就返回，如果没有下一个，那就返回 -1, -1
可能的数，就是列出这个格子所在行、列、九宫格，然后用 `Python集合` 去做减法运算，除去重复项，剩下的就是可能项
`block_in_grid` 和 `grid_get_block` 两个字典就可以很方便地查询给出九宫格序号，返回格子坐标列表，或者给出格子坐标，返回所在的九宫格序号

## 结尾语
这个数独的解法，最初写出来时候，在尝试函数处，其代码如下

    for i in range(1, 10):  # 从可能的数字中尝试

当然是在没有写出`possible_num`这个函数之前
如果从1到9遍历，查看最终结果，原本只需尝试80多次，现在需要尝试800多次，就做了很多不必要的尝试及回溯。
这就是这个算法的核心优化