---
title: 簡明 APCS 大學程式設計先修檢測入門教學
date: 2019-12-15 19:32:59
tags:
author: KD
---

# 前言

[APCS](https://apcs.csie.ntnu.edu.tw/)（Advanced Placement Computer Science，大學程式設計先修檢測入門教學）是教育部所推動的資訊能力檢測，希望藉由舉辦具公信力之「程式設計檢測」，讓具備程式設計能力之高中職學生，能夠檢驗學習成果，並供作大學選才的參考依據（可以想成是類似程式設計版本的全民英檢）。即便引起許多討論和爭議（軟體開發重點在於透過程式語言這項工具解決生活上的問題，過分強調程式解題有可能會抹煞學生學習程式設計的熱情或是城鄉師資差距、學生家長的焦慮等），但目前已有許多大學資工資管等相關資訊科系開設相關組別或是名額採計檢定成績。本篇文章透過 APCS 釋出的過去歷屆考題來介紹 APCS 相關資訊和觀念。

# APCS 簡介

APCS 檢定試題共分為兩個部分：程式設計觀念和程式設計實作，兩者皆為中文命題，採線上方式進行測驗。

## 考試資訊

程式設計觀念題共有 40 題，分兩節測驗（各 20 題，各 60 分鐘），程式設計實作題共有 4 個題組（150 分鐘）。APCS 每年固定有幾個月時間會有考試檢定，在許多縣市皆有設置考場，推廣期間免收報名費。詳細考試檢定資訊以[官方網站](https://apcs.csie.ntnu.edu.tw/)為主。

![](https://static.coderbridge.com/img/kdchang/5260e2cde29d4e219dcae286d4d58c76.png)

## 成績計算方式

以下為分數與級別對照表：

![](https://static.coderbridge.com/img/kdchang/c197b8e9b6b149ec99b72f77bfdf1cfb.png)

## 測試環境

在 APCS 官方網站有提供[實作練習環境](https://apcs.csie.ntnu.edu.tw/index.php/info/environment/)可以下載使用（需安裝 [VirtualBox](https://www.virtualbox.org/)），按照步驟指示可以安裝 Ubuntu 的系統環境（裡面已經安裝好 Eclipse、Code Block、Python2/3 IDLE 等工具）。

![](https://static.coderbridge.com/img/kdchang/d6a5be54e8734cf5a178d6cfe2370495.png)

# 程式設計觀念題

程式設計觀念主要是單選題命題，以運算思維、問題解決與程式設計觀念測驗為主。測驗內容包含：`code tracing`、`code debugging` 和 `code completion` 等類型（程式片段以 C 語言命題）。

程式設計觀念內容領域包含：

1. 程式設計基本觀念
2. 資料型別、變數（全域變數、區域變數）、常數
3. 條件控制
4. 迴圈
5. 函式
6. 陣列、結構等資料結構
7. 遞迴

以下我們根據官網提供的歷屆試題，用兩個範例來讓讀者感受一下程式設計觀念題。

![](https://static.coderbridge.com/img/kdchang/f96f3969df6b476395945fc6ef04f81f.png)

本題參考解答為：`D`，主要是測驗循序搜尋法及二分搜尋演算法的比較。

解題思路：

`value = 100` 代表 `k` 為 33，循序從 0 到 33 需執行 34 次。

若透過二分搜尋算法，每次對切會更逼近答案，可以在第 5 次時找到。

|      | low | high | mid |
| ---- | --- | ---- | --- |
| No.1 | 0   | 99   | 49  |
| No.2 | 0   | 48   | 24  |
| No.3 | 25  | 48   | 36  |
| No.4 | 25  | 35   | 30  |
| No.5 | 31  | 35   | 33  |

接著我們再來看另外一個範例：

![](https://static.coderbridge.com/img/kdchang/87dcd51724c64252a6e9662569a636a5.png)

本題參考解答為：`C`。主要希望透過迴圈找到最大元素值的索引值。最大值為 9，但取最後一次值出現 9 的 `index` 為 7。

# 程式設計實作題

程式設計實作題主要以完成程式為主，可以自行選擇以 C、C++、Java 或 Python 撰寫程式。我們範例將以 Python 為主。

以下我們根據官網提供的歷屆試題，用一個範例來讓讀者感受一下程式設計實作題。

![](https://static.coderbridge.com/img/kdchang/4e4d2307cdd04276a8c192dd885fae77.png)

![](https://static.coderbridge.com/img/kdchang/bd94f1473e4f42f296ce9cd8d5e2c2d7.png)

本題解題思路主要分為：

1. 輸入資料前處理
2. 進行成績資料的排序
3. 判斷最低分是否及格：若及格代表全部人都及格，否則循序取出最高不及格分數
4. 判斷最高分是否不及格：若不及格則全班不及格，否則循序取出最低及格分數

```
while True:
    input_line_1 = input('請輸入學生人數：')
    if not input_line_1:
        print('請輸入正確學生人數')
        break
    try:
        student_number = int(input_line_1)
    except ValueError:
        print('請輸入正確學生人數')
        break
    input_line_2 = input('請輸入學生成績：')
    if not input_line_2:
        print('請輸入學生成績')
        break
    raw_score_list = input_line_2.split(' ')

    if len(raw_score_list) != student_number:
        print('請確認成績筆數是否正確')
        break

    # 由小到大排序
    sorted_score_list = sorted(raw_score_list)
    sorted_score_str = ' '.join(sorted_score_list)
    print(sorted_score_str)

    sorted_score_list = [int(value) for value in sorted_score_list]


    # 由於由小到大排序，若第一個值也及格代表全班都及格
    if sorted_score_list[0] >= 60:
        print('best case')
    else:
        # 循序取出最高不及格分數
        i = student_number - 1
        while sorted_score_list[i] >= 60:
            i -= 1
        print(sorted_score_list[i])

    # 由於由小到大排序，若最後一個成績（最高分）也不及格代表全班都不及格
    if sorted_score_list[-1] < 60:
        print('worst case')
    else:
        # 循序取出最低及格分數
        i = 0
        while sorted_score_list[i] < 60:
            i += 1
        print(sorted_score_list[i])
```

# 總結

以上簡單介紹了 APCS 大學程式設計先修檢測相關資訊。雖然 APCS 可以透過考試檢定方式具更具體化呈現學生對於程式解題的概念（畢竟考試測驗是成本最低也看似公平的檢驗能力方式），政府單位願意重視資訊科技教育也是好事一件，也讓專攻程式設計能力專長的學生有多一個升學的管道。但在考試領導教學的教育環境下，若是沒有很好的配套教學措施可能容易讓莘莘學子誤以為程式設計和軟體開發就是程式解題或是拚命刷題，從而放棄了對於程式設計的熱情就得不償失了（邏輯思考和解決問題能力很重要，但軟體開發最重要的還是人的部分以及跨領域溝通協調和整合）。

# 參考文件

1. [APCS 官方網站](https://apcs.csie.ntnu.edu.tw/)
