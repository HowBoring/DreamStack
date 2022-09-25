---
title: "Longest Palindromic Substring"
date: 2022-09-23T18:15:02Z
tags: ["LeetCode"]
---

## 5. Longest Palindromic Substring

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (32.37%) | 21627 |   1237   |

**Tags:**
string | dynamic-programming

---

Given a string `s`, return *the longest palindromic substring* in `s`.

A string is called a palindrome string if the reverse of that string is the same as the original string.

 

**Example 1:**

```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

**Example 2:**

```
Input: s = "cbbd"
Output: "bb"
```

 

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters.

## Solution

```c++
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>

using namespace std;

using BoolTable = vector<vector<bool>>;
using Coordinate = pair<int, int>;

class Solution {
public:
  string longestPalindrome(string s) {
    BoolTable table = BoolTable();
    const size_t len = s.length();
    table.resize(len);
    for (auto &&row : table) {
      row.resize(len);
    }

    auto rst = buildTable(table, s);
    auto longestCoor =
      max_element(rst.begin(), rst.end(), [](Coordinate a,  Coordinate b) {
        return (a.second - a.first) < (b.second - b.first);
      });

    return string(s.begin() + longestCoor->first,
                  s.begin() + longestCoor->second + 1);
  }

private:
  vector<Coordinate> buildTable(BoolTable &table, string &str) {
    auto palindromics = vector<Coordinate>();
    for (int j = 0; j < str.length(); j++) {
      table[j][j] = true;
      palindromics.push_back(Coordinate(j, j));
      for (int i = 0; i < j; i++) {
        if (i + 1 <= j - 1) {
          table[i][j] = table[i + 1][j - 1] && (str[i] == str[j]);
        } else if (i + 1 == j) {
          table[i][j] = str[i] == str[j];
        } else {
          table[i][j] = false;
        }

        if (table[i][j]) {
          palindromics.push_back(Coordinate(i, j));
        }
      }
    }

    return palindromics;
  }
};
```
