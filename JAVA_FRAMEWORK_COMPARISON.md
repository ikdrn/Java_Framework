# Javaフレームワーク比較ガイド（非エンジニア〜エキスパート向け）

## 目次
- [1. まず3分で分かる：フレームワークとは？](#1-まず3分で分かるフレームワークとは)
- [2. Javaフレームワーク全体マップ（歴史〜最新）](#2-javaフレームワーク全体マップ歴史最新)
- [3. 主要フレームワーク比較表](#3-主要フレームワーク比較表)
- [4. 歴史と設計思想（なぜ生まれたか）](#4-歴史と設計思想なぜ生まれたか)
- [5. ディレクトリ構造比較（Vanilla / Spring / Spring Boot / Struts / Quarkus）](#5-ディレクトリ構造比較vanilla--spring--spring-boot--struts--quarkus)
- [6. 共通ユースケースのコード比較（GET /hello）](#6-共通ユースケースのコード比較get-hello)
- [7. どれを選ぶべきか（役割別おすすめ）](#7-どれを選ぶべきか役割別おすすめ)
- [8. シニア向け深掘り（起動・メモリ・内部構造）](#8-シニア向け深掘り起動メモリ内部構造)
- [9. まとめ](#9-まとめ)

---

## 1. まず3分で分かる：フレームワークとは？

### 非エンジニア向け（家づくりの例え）
フレームワークは、**「家づくりの標準キット」**です。

- フレームワークなし：毎回、設計図・配線・配管をゼロから決める
- フレームワークあり：標準の設計と部品があり、早く・安全に建てられる

#### ビジネスへの効果
- **開発スピード向上**：市場投入が早い
- **品質の安定**：人が変わっても品質がぶれにくい
- **保守性向上**：引き継ぎしやすく、改修コストを抑えやすい

### 初学者向け（なぜ使う？）
フレームワークを使うと、次を「自作」しなくてよくなります。
- URLごとの処理分岐（ルーティング）
- 依存関係の管理（DI）
- 設定ファイルの読み込み
- ログ、認証、テスト支援

### エキスパート向け（要点）
Javaは以下の方向で進化してきました。
1. **標準化重視**（Java EE / Jakarta EE）
2. **開発しやすさ重視**（Spring）
3. **初期構築の省力化**（Spring Boot）
4. **クラウド最適化**（Quarkus / Micronaut）
5. **高並行・非同期処理**（Vert.x / Play）

---

## 2. Javaフレームワーク全体マップ（歴史〜最新）

### 2.1 伝統的MVC / エンタープライズ
- Struts 1/2
- JSF（Jakarta Faces）
- Jakarta EE（JAX-RS, CDI, JPA など）
- Spring Framework
- Spring Boot

### 2.2 モダン・クラウドネイティブ
- Quarkus
- Micronaut
- Helidon
- Dropwizard
- Javalin
- Spark Java

### 2.3 リアクティブ / 非同期I/O
- Vert.x
- Play Framework
- Ratpack
- Akka HTTP（Java利用可）

### 2.4 API実装・統合系
- Jersey（JAX-RS実装）
- RESTEasy（JAX-RS実装）
- Apache CXF（REST/SOAP）
- Apache Camel（システム連携）

> 実務では「1つだけ選ぶ」というより、Spring Boot + Data + Security のように組み合わせるケースが一般的です。

---

## 3. 主要フレームワーク比較表

> ※性能は実装・JDK・GC・依存関係で変わるため、一般的な傾向です。

| フレームワーク | 主な特徴 / 設計思想 | 得意分野 | パフォーマンス特性（起動速度・リソース） | 学習コスト |
|---|---|---|---|---|
| Spring Framework | DI/IoC中心。モジュールが豊富 | 大規模業務、複雑な要件 | 起動:中〜遅 / メモリ:中〜高 | 中〜高 |
| Spring Boot | Auto ConfigurationとStarterで高速開発 | Web API、社内業務、マイクロサービス | 起動:中 / メモリ:中 | 低〜中 |
| Jakarta EE | 標準仕様準拠を重視（ベンダー選択しやすい） | 長期運用、標準準拠案件 | 実装依存（近年軽量化） | 中 |
| Struts 2 | Action + XMLの明示設定 | 既存レガシー保守 | 起動:中 / メモリ:中 | 中〜高 |
| Quarkus | Kubernetes/Native前提の設計 | クラウド、サーバーレス | 起動:速い / メモリ:低 | 中 |
| Micronaut | コンパイル時DIで軽量化 | マイクロサービス、Functions | 起動:速い / メモリ:低 | 中 |
| Vert.x | イベントループ中心、非同期I/Oに強い | 高並行API、イベント駆動 | 起動:速い / メモリ:低〜中 | 中〜高 |
| Play | ルーティング分離 + 非同期実行 | リアルタイムWeb、高並行処理 | 起動:中 / 実行性能:良好 | 中 |
| Dropwizard | REST APIを素早く構築する実務志向 | シンプルなAPI基盤 | 起動:速め / メモリ:中 | 低〜中 |
| Helidon | MicroProfile対応、Oracle系 | クラウド向けサービス | 起動:速め / メモリ:低〜中 | 中 |
| Javalin | 超軽量で学びやすい | 小〜中規模API、PoC | 起動:速い / メモリ:低 | 低 |
| Spark Java | 最小DSLで実装可能 | 学習用途、小規模サービス | 起動:速い / メモリ:低 | 低 |
| Jersey / RESTEasy | JAX-RS標準REST実装 | 標準準拠REST API | 実装依存で中 | 中 |
| Apache CXF | SOAP/REST統合を重視 | 企業間連携、レガシー連携 | 中 | 中〜高 |
| Ratpack | 非同期・軽量HTTP基盤 | 高性能バックエンド | 起動:速め / メモリ:低〜中 | 中 |

---

## 4. 歴史と設計思想（なぜ生まれたか）

### 4.1 Struts・初期Java EE
- 当時の目的：Web開発を統一的に進める
- 代表的な特徴：XML設定中心、構成が明示的
- 課題：設定が増えると変更が重くなる

### 4.2 Spring
- 背景：重量級コンポーネントモデル（EJB）への反動
- 狙い：POJO + DIで柔軟・テストしやすい設計
- 結果：企業Javaの中心技術に

### 4.3 Spring Boot
- 背景：マイクロサービス時代の「早く作って出す」要求
- 狙い：自動設定で初期構築を大幅削減
- 結果：最も採用されるJavaアプリ開発手法の1つに

### 4.4 Quarkus / Micronaut
- 背景：コンテナ常駐・スケール前提の運用
- 狙い：高速起動、低メモリ、クラウド効率
- キーワード：AOT、コンパイル時解析、Native Image

### 4.5 Vert.x / Play
- 背景：同時接続増加と低遅延要求
- 狙い：ノンブロッキングI/Oで高並行処理
- 注意点：設計・障害解析の難易度は上がりやすい

---

## 5. ディレクトリ構造比較（Vanilla / Spring / Spring Boot / Struts / Quarkus）

ここでは「設定ファイルがどこにあるか」「規約の強さ」を見える化します。

### 5.1 Vanilla Java（フレームワークなし）
```text
vanilla-java-app/
├── src/
│   ├── main/java/com/example/App.java
│   └── test/java/com/example/AppTest.java
├── pom.xml (or build.gradle)
└── README.md
```
- 設定場所：主に `pom.xml` / `build.gradle`
- 規約：最低限のみ。自由度は高いが、機能を自作しがち

### 5.2 Spring Framework（非Boot）
```text
spring-framework-app/
├── src/main/java/com/example/
│   ├── config/AppConfig.java
│   ├── controller/
│   ├── service/
│   └── repository/
├── src/main/resources/
│   ├── application.properties
│   └── spring/ (XML利用時)
├── src/main/webapp/WEB-INF/
│   ├── web.xml
│   └── dispatcher-servlet.xml (XML利用時)
└── pom.xml
```
- 設定場所：`web.xml`、`dispatcher-servlet.xml`、`application.properties`
- 規約：Bootより自由度が高く、初期設定は多め

### 5.3 Spring Boot
```text
spring-boot-app/
├── src/main/java/com/example/
│   ├── Application.java
│   ├── controller/
│   ├── service/
│   └── repository/
├── src/main/resources/
│   ├── application.properties (or application.yml)
│   ├── static/
│   └── templates/
├── src/test/java/com/example/
└── pom.xml
```
- 設定場所：主に `application.properties`
- 規約：**Convention over Configuration**が強く、定位置に置くと設定量が減る
- 補足：`web.xml` 不要（組み込みサーバーで起動）

### 5.4 Struts 2
```text
struts2-app/
├── src/main/java/com/example/
│   ├── action/
│   ├── service/
│   └── model/
├── src/main/resources/struts.xml
├── src/main/webapp/
│   ├── WEB-INF/web.xml
│   └── jsp/
└── pom.xml
```
- 設定場所：`struts.xml` と `web.xml`
- 規約：明示設定中心（追跡しやすい反面、記述量は増える）

### 5.5 Quarkus
```text
quarkus-app/
├── src/main/java/org/acme/GreetingResource.java
├── src/main/resources/
│   ├── application.properties
│   └── META-INF/resources/
├── src/main/docker/
├── src/test/java/org/acme/
└── pom.xml
```
- 設定場所：主に `application.properties`
- 規約：拡張（extension）追加で機能を増やす
- 補足：Docker関連ディレクトリを初期に持つことが多い

### 5.6 規約の違い（ひと目で）
- **Vanilla Java**：規約弱い（自由だが自作増える）
- **Spring Framework**：中程度（選択肢が多い）
- **Spring Boot**：規約強い（早い）
- **Struts**：明示設定強い（古典的で読みやすい）
- **Quarkus**：クラウド実行に寄せた規約

---

## 6. 共通ユースケースのコード比較（GET /hello）

### ユースケース
`GET /hello` で `Hello, Java Framework!` を返す。

### 6.1 Vanilla Java（JDK簡易HTTPサーバー）
```java
import com.sun.net.httpserver.HttpServer;
import java.io.OutputStream;
import java.net.InetSocketAddress;

public class App {
    public static void main(String[] args) throws Exception {
        HttpServer server = HttpServer.create(new InetSocketAddress(8080), 0);
        server.createContext("/hello", exchange -> {
            String response = "Hello, Java Framework!";
            exchange.getResponseHeaders().add("Content-Type", "text/plain; charset=UTF-8");
            exchange.sendResponseHeaders(200, response.getBytes().length);
            try (OutputStream os = exchange.getResponseBody()) {
                os.write(response.getBytes());
            }
        });
        server.start();
    }
}
```
- なぜこうなる？：最低限の標準APIだけで実装するため
- 長所：仕組み理解に最適
- 短所：実務機能（DI、認証、監視）を自作しがち

### 6.2 Spring Boot（アノテーションベース）
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

@RestController
class HelloController {
    @GetMapping("/hello")
    public String hello() {
        return "Hello, Java Framework!";
    }
}
```
- なぜこうなる？：宣言（アノテーション）でルーティングを表現
- 長所：少ない記述で本番レベル機能に届きやすい
- 短所：内部の自動設定を理解するまでブラックボックス感がある

### 6.3 Spring Framework（非Boot）
```java
import org.springframework.context.annotation.Configuration;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
@EnableWebMvc
class WebConfig implements WebMvcConfigurer {
    // Bootより明示設定が必要
}

@Controller
class HelloController {
    @GetMapping("/hello")
    @ResponseBody
    public String hello() {
        return "Hello, Java Framework!";
    }
}
```
- なぜこうなる？：柔軟性を優先し、設定を明示できるため
- 長所：細かな制御がしやすい
- 短所：初期構成の手間はBootより大きい

### 6.4 Struts 2（Action + XML）
```java
import com.opensymphony.xwork2.ActionSupport;

public class HelloAction extends ActionSupport {
    private String message;

    @Override
    public String execute() {
        message = "Hello, Java Framework!";
        return SUCCESS;
    }

    public String getMessage() {
        return message;
    }
}
```

```xml
<!-- struts.xml -->
<struts>
    <package name="default" namespace="/" extends="struts-default">
        <action name="hello" class="com.example.action.HelloAction">
            <result>/jsp/hello.jsp</result>
        </action>
    </package>
</struts>
```
- なぜこうなる？：ルーティングと遷移先をXMLで明示管理する設計
- 長所：設定ファイルを読めば挙動を追いやすい
- 短所：記述量と設定保守の負担が増えやすい

### 6.5 Jakarta EE（JAX-RS）
```java
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;

@Path("/hello")
public class HelloResource {
    @GET
    @Produces(MediaType.TEXT_PLAIN)
    public String hello() {
        return "Hello, Java Framework!";
    }
}
```
- なぜこうなる？：標準仕様（JAX-RS）に沿ってRESTを定義するため
- 長所：ベンダー依存を抑えやすい
- 短所：実装や運用基盤の選定が別途必要

### 6.6 Quarkus（JAX-RS + クラウド最適化）
```java
import jakarta.ws.rs.GET;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;

@Path("/hello")
public class GreetingResource {
    @GET
    @Produces(MediaType.TEXT_PLAIN)
    public String hello() {
        return "Hello, Java Framework!";
    }
}
```
- なぜこうなる？：標準的な記法を保ちながら実行基盤で軽量化するため
- 長所：高速起動・低メモリに寄せやすい
- 短所：Native運用ではビルドと検証設計が必要

### 6.7 Micronaut（コンパイル時DI）
```java
import io.micronaut.http.annotation.Controller;
import io.micronaut.http.annotation.Get;

@Controller
public class HelloController {
    @Get("/hello")
    public String hello() {
        return "Hello, Java Framework!";
    }
}
```
- なぜこうなる？：アノテーションをコンパイル時解析し実行時負荷を抑えるため
- 長所：起動が速く、軽量
- 短所：Spring流儀と細部が異なり学習の切り替えが必要

### 6.8 Vert.x（関数型ルーティング）
```java
import io.vertx.core.AbstractVerticle;
import io.vertx.core.Promise;
import io.vertx.ext.web.Router;

public class MainVerticle extends AbstractVerticle {
    @Override
    public void start(Promise<Void> startPromise) {
        Router router = Router.router(vertx);
        router.get("/hello").handler(ctx -> ctx.response().end("Hello, Java Framework!"));

        vertx.createHttpServer()
             .requestHandler(router)
             .listen(8080)
             .onSuccess(server -> startPromise.complete())
             .onFailure(startPromise::fail);
    }
}
```
- なぜこうなる？：イベントループのノンブロッキングI/Oが中心だから
- 長所：高並行アクセスに強い
- 短所：スレッド/非同期設計の理解が必要

### 6.9 Play Framework（routes + Controller）
```text
# conf/routes
GET   /hello   controllers.HelloController.hello
```

```java
package controllers;

import play.mvc.Controller;
import play.mvc.Result;

public class HelloController extends Controller {
    public Result hello() {
        return ok("Hello, Java Framework!");
    }
}
```
- なぜこうなる？：ルート定義と処理コードを分離する思想のため
- 長所：責務分離が明確で見通しがよい
- 短所：フレームワーク固有ルールの把握が必要

---

## 7. どれを選ぶべきか（役割別おすすめ）

### 7.1 非エンジニア（意思決定者）
**まずこの3軸で判断**
1. どれだけ早く出したいか（Time to Market）
2. 将来の保守コストを下げられるか
3. 採用しやすい技術か（人材市場）

**実務の第一候補**
- 多くの企業で無難：**Spring Boot**
- 標準仕様を重視：**Jakarta EE**
- クラウド効率重視：**Quarkus / Micronaut**

### 7.2 初学者
- 1本目：**Spring Boot**（情報量が多く学びやすい）
- 2本目：**Jakarta EE（JAX-RS）**で標準を理解
- 3本目：**Vert.x または Quarkus**で非同期/クラウドを体験

### 7.3 シニアエンジニア
- 重視すべき観点
  - P99レイテンシ
  - 起動時間（オートスケール時の効き）
  - メモリ上限（コンテナ密度）
  - 観測性（ログ/メトリクス/トレース）
  - チーム運用能力

---

## 8. シニア向け深掘り（起動・メモリ・内部構造）

### 8.1 起動時間とメモリ
- Spring Boot：生産性が高く総合力が高い
- Quarkus/Micronaut：軽量性を得やすい
- Native Image：高速起動に強いが、ビルド戦略が重要

### 8.2 DIの実行時解析 vs コンパイル時解析
- 実行時解析（代表：Spring）：柔軟、エコシステム巨大
- コンパイル時解析（代表：Micronaut）：起動/メモリに有利

### 8.3 リアクティブ設計の注意
- 効果：I/O待ちが多いAPIで効率が上がりやすい
- 落とし穴：ブロッキング処理の混在で性能が崩れる
- 対策：タイムアウト、リトライ、バルクヘッド、サーキットブレーカを最初から設計

### 8.4 レガシー移行の実践
Strutsなどの既存資産は「段階移行」が安全です。
1. まずAPI境界を明確化
2. 新機能をSpring Boot / Quarkusで新設
3. 旧機能を順次置換

---

## 9. まとめ

- **迷ったら Spring Boot**（実績・人材・情報が豊富）
- **標準準拠を重視するなら Jakarta EE**
- **起動速度/メモリを重視するなら Quarkus / Micronaut**
- **高並行I/Oが主戦場なら Vert.x / Play**
- **既存資産保守では Struts の理解が今も価値を持つ**

最適解は1つではありません。
**「ビジネス要件 × 非機能要件 × チーム成熟度」**で選ぶのが正解です。
