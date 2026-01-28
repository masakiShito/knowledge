# Javaのナレッジ（心得 + 構文てんこ盛り）

## 心得（考え方・設計）
- **読みやすさと保守性が最優先**: 明示的で一貫した構造が武器。
- **OOPの基本に忠実**: カプセル化・継承・ポリモーフィズムを意識。
- **SOLIDを意識**: 特に単一責任・依存性逆転。
- **例外は仕様**: 想定内の例外は適切にハンドリング。
- **イミュータブルを推奨**: 可変はバグの温床。
- **標準ライブラリ優先**: `java.*` / `javax.*` をまず検討。
- **性能より正しさ**: 正しく動かしてから最適化。
- **テストで仕様を固定**: テストは将来の自分への説明書。
- **ツールチェーン重視**: ビルド・Lint・CIを整える。

---

## 1. 基本文法
### 1.1 Hello World
```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, Java");
    }
}
```

### 1.2 変数・型
```java
int x = 10;
long big = 123456789L;
float f = 3.14f;
double d = 3.14159;
char c = 'A';
boolean flag = true;
String s = "hello";
```

### 1.3 キャスト
```java
int a = (int) 3.14; // 明示的キャスト
long b = 10;        // 暗黙キャスト
```

---

## 2. 演算子
```java
int sum = 1 + 2;
int mod = 10 % 3;
boolean ok = (sum > 2) && (mod == 1);
```

---

## 3. 制御構文
### 3.1 if / else
```java
if (x > 0) {
    System.out.println("positive");
} else if (x == 0) {
    System.out.println("zero");
} else {
    System.out.println("negative");
}
```

### 3.2 switch
```java
switch (x) {
    case 1 -> System.out.println("one");
    case 2 -> System.out.println("two");
    default -> System.out.println("other");
}
```

### 3.3 for / while / do-while
```java
for (int i = 0; i < 3; i++) {
    System.out.println(i);
}

int n = 3;
while (n > 0) {
    n--;
}

do {
    n++;
} while (n < 3);
```

### 3.4 break / continue
```java
for (int i = 0; i < 5; i++) {
    if (i == 3) break;
    if (i == 1) continue;
}
```

---

## 4. 配列
```java
int[] arr = {1, 2, 3};
int[][] matrix = {{1, 2}, {3, 4}};

System.out.println(arr[0]);
```

---

## 5. 文字列
```java
String s = "Java";
System.out.println(s.length());
System.out.println(s.substring(1, 3));
System.out.println(s.toUpperCase());
```

### 5.1 StringBuilder
```java
StringBuilder sb = new StringBuilder();
sb.append("hello").append(" world");
System.out.println(sb.toString());
```

---

## 6. メソッド
```java
public static int add(int a, int b) {
    return a + b;
}

// 可変長引数
public static int sum(int... nums) {
    int total = 0;
    for (int n : nums) total += n;
    return total;
}
```

---

## 7. クラスとOOP
### 7.1 基本クラス
```java
class User {
    private String name;

    public User(String name) {
        this.name = name;
    }

    public String greet() {
        return "Hi " + name;
    }
}
```

### 7.2 継承
```java
class Admin extends User {
    public Admin(String name) {
        super(name);
    }
}
```

### 7.3 抽象クラスとインターフェース
```java
abstract class Animal {
    abstract void sound();
}

interface Runnable {
    void run();
}
```

---

## 8. アクセス修飾子
- `public`: どこからでも
- `protected`: 同一パッケージ + サブクラス
- `private`: クラス内のみ
- （修飾子なし）: パッケージプライベート

---

## 9. パッケージ / import
```java
package com.example.app;

import java.util.List;
```

---

## 10. コレクション
### 10.1 List / Set / Map
```java
List<String> list = new ArrayList<>();
Set<Integer> set = new HashSet<>();
Map<String, Integer> map = new HashMap<>();
```

### 10.2 反復処理
```java
for (String v : list) {
    System.out.println(v);
}

list.forEach(System.out::println);
```

---

## 11. ジェネリクス
```java
List<String> names = new ArrayList<>();

public static <T> T first(List<T> list) {
    return list.get(0);
}
```

---

## 12. 例外処理
```java
try {
    int x = 1 / 0;
} catch (ArithmeticException e) {
    e.printStackTrace();
} finally {
    System.out.println("always");
}
```

### 12.1 checked / unchecked
- **checked例外**: `IOException`
- **unchecked例外**: `RuntimeException`

---

## 13. 文字入出力 / ファイル操作
```java
import java.nio.file.*;

Path path = Paths.get("data.txt");
Files.writeString(path, "hello");
String content = Files.readString(path);
```

---

## 14. 型変換とラッパークラス
```java
int n = Integer.parseInt("10");
String s = String.valueOf(123);
Integer obj = Integer.valueOf(5);
```

---

## 15. 日付と時刻
```java
import java.time.*;

LocalDate date = LocalDate.now();
LocalDateTime dt = LocalDateTime.now();
```

---

## 16. Stream API
```java
List<Integer> nums = List.of(1, 2, 3, 4);
int sum = nums.stream()
    .filter(n -> n % 2 == 0)
    .mapToInt(n -> n)
    .sum();
```

### 16.1 Optional
```java
Optional<String> name = Optional.of("Alice");
name.ifPresent(System.out::println);
```

---

## 17. 関数型インターフェース / ラムダ
```java
Function<Integer, Integer> f = x -> x * 2;
Predicate<String> p = s -> s.isEmpty();
Consumer<String> c = System.out::println;
```

---

## 18. スレッド / 並行処理
```java
Thread t = new Thread(() -> System.out.println("run"));
t.start();
```

### 18.1 ExecutorService
```java
ExecutorService executor = Executors.newFixedThreadPool(2);
executor.submit(() -> System.out.println("task"));
executor.shutdown();
```

---

## 19. 同期 / ロック
```java
synchronized void inc() {
    // 排他制御
}
```

---

## 20. JVMまわり
- **JDK vs JRE**: 開発環境と実行環境
- **GC**: ガーベジコレクションで自動メモリ管理
- **JIT**: 実行時に最適化

---

## 21. ビルドツール
- **Maven** / **Gradle** が主流
- `pom.xml` / `build.gradle` で依存管理

---

## 22. テスト
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

@Test
void testAdd() {
    assertEquals(2, 1 + 1);
}
```

---

## 23. ログ
```java
import java.util.logging.Logger;

Logger log = Logger.getLogger("app");
log.info("hello");
```

---

## 24. Javaのよくある罠
- `==` と `.equals()` の混同
- `null`チェック不足
- `String`結合で `+` を多用（ループ内は`StringBuilder`推奨）
- `hashCode()` / `equals()` の不整合

---

## 25. 設計パターン（定番）
- Singleton / Factory / Builder
- Strategy / Observer / Decorator
- Adapter / Facade

---

## 26. モダンJava機能（Java 8以降）
- **var**（ローカル型推論）
- **record**（イミュータブルデータ）
- **sealed class**（継承制限）
- **text block**（複数行文字列）

---

## 27. 依存管理と環境
- `sdkman` でJDK管理
- `gradle wrapper` / `mvnw` 推奨

---

## 28. CLI実行例
```bash
javac Main.java
java Main
```

---

## 29. もっと深めたいトピック
- JVMチューニング
- Bytecode解析
- Spring / Jakarta EE
- ネットワーク（NIO / Netty）
- プロファイリング（JFR / VisualVM）

---

## まとめ
Javaは「堅牢さ」「スケーラビリティ」「長期運用」を前提にした言語。基本構文に加えて、OOP・標準ライブラリ・ツールチェーン・並行処理まで押さえると強い。
