## 心得（押さえるべき考え方）
- **読みやすさ優先**: 「読みやすさは正義」。短くても読みにくいコードより、明瞭なコード。
- **単一責任**: 1関数1目的。長大な関数は分割する。
- **明示は暗黙に勝る**: マジックに見える挙動を避け、意図をコードで表現する。
- **イミュータブルを優先**: 可能ならタプルや`frozenset`など不変を使う。
- **まずは正しく、次に速く**: 早すぎる最適化はバグ源。
- **テストは仕様**: テストが使い方と期待を表す。
- **標準ライブラリは宝庫**: まず標準ライブラリを調べる。
- **型ヒントで意図を伝える**: 可読性と保守性が上がる。
- **例外で早期失敗**: 異常系は早めに落とす。

---

## 1. 基本構文
### 1.1 コメント
```py
# 1行コメント
"""複数行コメント風（実際は文字列）"""
```

### 1.2 変数と代入
```py
x = 10
name = "Alice"
pi = 3.14159

# 多重代入
x, y, z = 1, 2, 3

# スワップ
x, y = y, x
```

### 1.3 文字列（f-string推奨）
```py
name = "Bob"
age = 20
msg = f"{name} is {age}"

# 文字列操作
s = "python"
print(s.upper())   # PYTHON
print(s[:3])       # pyt
print(s.replace("py", "PY"))
```

---

## 2. 型とデータ構造
### 2.1 基本型
```py
num = 42        # int
pi = 3.14       # float
flag = True     # bool
text = "hi"     # str
```

### 2.2 リスト / タプル / セット / 辞書
```py
lst = [1, 2, 3]
tpl = (1, 2, 3)
set_ = {1, 2, 3}

d = {"a": 1, "b": 2}

# 要素追加
lst.append(4)
set_.add(4)
d["c"] = 3
```

### 2.3 内包表記（comprehension）
```py
squares = [x * x for x in range(5)]
keys = {k.upper() for k in d.keys()}
inv = {v: k for k, v in d.items()}
```

---

## 3. 制御構文
### 3.1 if / elif / else
```py
if x > 0:
    print("positive")
elif x == 0:
    print("zero")
else:
    print("negative")
```

### 3.2 for / while
```py
for i in range(3):
    print(i)

n = 3
while n > 0:
    n -= 1
```

### 3.3 break / continue / else
```py
for i in range(5):
    if i == 3:
        break
else:
    print("breakされなかった時だけ実行")
```

---

## 4. 関数
### 4.1 定義と引数
```py
def add(a, b=0):
    return a + b

# 可変長引数

def foo(*args, **kwargs):
    print(args, kwargs)
```

### 4.2 ラムダ式
```py
f = lambda x: x * 2
print(f(5))
```

### 4.3 関数を引数に
```py
def apply(func, x):
    return func(x)

apply(lambda v: v + 1, 10)
```

---

## 5. 例外処理
```py
try:
    x = 1 / 0
except ZeroDivisionError:
    print("割り算エラー")
else:
    print("例外なし")
finally:
    print("必ず実行")
```

---

## 6. イテレータ / ジェネレータ
```py
# ジェネレータ

def count_up(n):
    i = 0
    while i < n:
        yield i
        i += 1

for v in count_up(3):
    print(v)
```

---

## 7. モジュールとパッケージ
```py
# import
import math
from datetime import datetime

print(math.sqrt(16))
print(datetime.now())
```

---

## 8. クラスとOOP
```py
class User:
    def __init__(self, name):
        self.name = name

    def greet(self):
        return f"Hi {self.name}"

u = User("Alice")
print(u.greet())
```

### 8.1 dataclasses
```py
from dataclasses import dataclass

@dataclass
class Point:
    x: int
    y: int

p = Point(1, 2)
```

---

## 9. 型ヒント
```py
from typing import List, Dict

def total(nums: List[int]) -> int:
    return sum(nums)

users: Dict[str, int] = {"a": 1}
```

---

## 10. ファイル操作
```py
with open("data.txt", "w", encoding="utf-8") as f:
    f.write("hello")

with open("data.txt", "r", encoding="utf-8") as f:
    content = f.read()
```

---

## 11. 標準ライブラリの重要どころ
- **collections**: `Counter`, `defaultdict`, `deque`
- **itertools**: `product`, `permutations`, `groupby`
- **functools**: `lru_cache`, `partial`
- **pathlib**: パス操作の基本
- **logging**: ログ管理
- **json / csv**: データ読み書き

---

## 12. 非同期処理（asyncio）
```py
import asyncio

async def main():
    await asyncio.sleep(1)
    return "done"

print(asyncio.run(main()))
```

---

## 13. テスト
```py
# pytest 例

def test_add():
    assert 1 + 1 == 2
```

---

## 14. コーディング規約 / ツール
- **PEP8**: Pythonの公式スタイル
- **black**: 自動フォーマッタ
- **ruff / flake8**: Lint
- **mypy / pyright**: 型チェック

---

## 15. よくある罠
- `list = []` などの**組み込み名を上書き**しない
- デフォルト引数に**ミュータブル**を置かない
```py
# NG

def f(x, lst=[]):
    lst.append(x)
    return lst
```

- `is` と `==` の混同

---

## 16. パフォーマンス小ネタ
- 文字列結合は `"".join(list)`
- ループより内包表記が速い場合が多い
- `lru_cache` でメモ化

---

## 17. 依存管理・環境
- `venv`で仮想環境
- `pip`でパッケージ管理
- `pip-tools`/`poetry`/`uv` なども選択肢

---

## 18. CLIスクリプト
```py
import sys

if __name__ == "__main__":
    print(sys.argv)
```

---

## 19. もっと深めたいトピック
- 並行処理（threading / multiprocessing）
- パッケージ配布（setup.py / pyproject.toml）
- C拡張 / Cython
- プロファイリング（cProfile）

---

## まとめ
Pythonは「読みやすさ」を中心に据えた言語。基本構文・標準ライブラリ・ツールを押さえることで、短く安全で保守しやすいコードが書ける。
