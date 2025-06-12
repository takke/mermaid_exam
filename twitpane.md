# TwitPane のアーキテクチャ変遷

- ViewModel, UseCase, Repository はこんなに綺麗に Clean Architecture で分離されていない


## 2025年後半: TwitPane, ついぺんリサーチR, ZonePane, たいぺん, BluePane
- CMP対応？
 
```mermaid
flowchart TD
  %% Android Apps subgraph
  subgraph AAP["🤖 Android Apps"]
    TP["📱 TwitPane"]
    TPR["📱 ついぺん
リサーチR"]
    ZP["📱 ZonePane"]
    TAP["📱 たいぺん"]
    BP["📱 BluePane"]
    ZPZ_A["📱 ZonePane Zero
(Android)"]
  end

  %% CMP Apps subgraph
  subgraph CAP["🧪 CMP Apps"]
    ZPZ_I["📱 ZonePane Zero
(iOS)"]
  end

  %% UI Layer
  UI["🧑‍💻 UI (Activity / Fragment / Compose(Bluesky, タイッツー))"]
  UIC["🧑‍💻 UI (Compose Multiplatform)"]

  %% Logic Layer subgraph
  subgraph LG["🧠 Logic Layer"]
    VMA["🧠 ViewModel(Android専用)"]
    VMC["🧠 ViewModel(CMP対応)"]
    UCA["🧭 UseCase(Android専用)"]
    UCC["🧭 UseCase(CMP対応)"]
    RPA["📦 Repository(Android専用)"]
    RPC["📦 Repository(CMP対応)"]
  end

  %% DataSource subgraph
  subgraph DS["🗄️ DataStore"]
    TW["🐦 Twitter4J"]
    MA["🐘 mastodon4j"]
    MI["✨ misskey4j"]
    KB["🌀 kbsky"]
    TT["🧦 taittsuu4j"]
    DB["💾 Local DB, Preferences"]
  end

  %% Service subgraph
  subgraph SV["🌍 Services"]
    S_TW["Twitter"]
    S_MA["Mastodon"]
    S_MI["Misskey"]
    S_KB["Bluesky"]
    S_TT["タイッツー"]
  end

  %% App connections
  TP --> UI
  TPR --> UI
  ZP --> UI
  TAP --> UI
  BP --> UI
  ZPZ_A --> UIC
  ZPZ_I --> UIC

  %% UI to ViewModel
  UI --> VMA
  UI --> VMC
  UIC --> VMC

  %% ViewModel to UseCase
  VMA --> UCA
  VMA --> UCC
  VMC --> UCC

  %% UseCase to Repository
  UCA --> RPA
  UCA --> RPC
  UCC --> RPC

  %% Repository to DataStore (1 line per repo)
  RPA --> DS
  RPC --> DS

  %% DataStore to Services
  TW --> S_TW
  MA --> S_MA
  MI --> S_MI
  KB --> S_KB
  TT --> S_TT

```


## 2025年前半: TwitPane, ついぺんリサーチR, ZonePane, たいぺん, BluePane
- BluePane(Bluesky専用)
- クロスポスト対応
 
```mermaid
flowchart TD
  %% Android Apps subgraph
  subgraph AAP["🤖 Android Apps"]
    TP["📱 TwitPane"]
    TPR["📱 ついぺんリサーチR"]
    ZP["📱 ZonePane"]
    TAP["📱 たいぺん"]
    BP["📱 **BluePane**"]
  end

  %% UI Layer
  UI["🧑‍💻 UI (Activity / Fragment / Compose(Bluesky, タイッツー))"]

  %% Logic Layer subgraph
  subgraph LG["🧠 Logic Layer"]
    VM["🧠 ViewModel(Compose対応)"]
    UC["🧭 UseCase"]
    RP["📦 Repository"]
  end

  %% DataSource subgraph
  subgraph DS["🗄️ DataStore"]
    TW["🐦 Twitter4J"]
    MA["🐘 mastodon4j"]
    MI["✨ misskey4j"]
    KB["🌀 kbsky"]
    TT["🧦 taittsuu4j"]
    DB["💾 Local DB, Preferences"]
  end

  %% Service subgraph
  subgraph SV["🌍 Services"]
    S_TW["Twitter"]
    S_MA["Mastodon"]
    S_MI["Misskey"]
    S_KB["Bluesky"]
    S_TT["タイッツー"]
  end

  %% App connections
  TP --> UI
  TPR --> UI
  ZP --> UI
  TAP --> UI
  BP --> UI

  %% UI to Logic
  UI --> VM --> UC --> RP

  %% Repository to DataSources
  RP --> DS

  %% DataStore to Services
  TW --> S_TW
  MA --> S_MA
  MI --> S_MI
  KB --> S_KB
  TT --> S_TT
```

## 2024年後半: TwitPane, ついぺんリサーチR, ZonePane, たいぺん
- タイッツー対応(たいぺん)

```mermaid
flowchart TD
  %% Android Apps subgraph
  subgraph AAP["🤖 Android Apps"]
    TP["📱 TwitPane"]
    TPR["📱 ついぺんリサーチR"]
    ZP["📱 ZonePane"]
    TAP["📱 **たいぺん**"]
  end

  %% UI Layer
  UI["🧑‍💻 UI (Activity / Fragment / Compose(Bluesky, タイッツー))"]

  %% Logic Layer subgraph
  subgraph LG["🧠 Logic Layer"]
    VM["🧠 ViewModel(Compose対応)"]
    UC["🧭 UseCase"]
    RP["📦 Repository"]
  end

  %% DataSource subgraph
  subgraph DS["🗄️ DataStore"]
    TW["🐦 Twitter4J"]
    MA["🐘 mastodon4j"]
    MI["✨ misskey4j"]
    KB["🌀 kbsky"]
    TT["🧦 **taittsuu4j**"]
    DB["💾 Local DB, Preferences"]
  end

  %% Service subgraph
  subgraph SV["🌍 Services"]
    S_TW["Twitter"]
    S_MA["Mastodon"]
    S_MI["Misskey"]
    S_KB["Bluesky"]
    S_TT["タイッツー"]
  end

  %% App connections
  TP --> UI
  TPR --> UI
  ZP --> UI
  TAP --> UI

  %% UI to Logic
  UI --> VM --> UC --> RP

  %% Repository to DataSources
  RP --> DS

  %% DataStore to Services
  TW --> S_TW
  MA --> S_MA
  MI --> S_MI
  KB --> S_KB
  TT --> S_TT
```

## 2024年前半: TwitPane, ついぺんリサーチR, ZonePane
- Bluesky対応
- Compose で実装

```mermaid
flowchart TD
  %% Android Apps subgraph
  subgraph AAP["🤖 Android Apps"]
    TP["📱 TwitPane"]
    TPR["📱 ついぺんリサーチR"]
    ZP["📱 ZonePane"]
  end

  %% UI Layer
  UI["🧑‍💻 UI (Activity / Fragment / Compose(Bluesky))"]

  %% Logic Layer subgraph
  subgraph LG["🧠 Logic Layer"]
    VM["🧠 ViewModel(Compose対応)"]
    UC["🧭 UseCase"]
    RP["📦 Repository"]
  end

  %% DataSource subgraph
  subgraph DS["🗄️ DataStore"]
    TW["🐦 Twitter4J"]
    MA["🐘 mastodon4j"]
    MI["✨ misskey4j"]
    KB["🌀 **kbsky**"]
    DB["💾 Local DB, Preferences"]
  end

  %% Service subgraph
  subgraph SV["🌍 Services"]
    S_TW["Twitter"]
    S_MA["Mastodon"]
    S_MI["Misskey"]
    S_KB["Bluesky"]
  end

  %% App connections
  TP --> UI
  TPR --> UI
  ZP --> UI

  %% UI to Logic
  UI --> VM --> UC --> RP

  %% Repository to DataSources
  RP --> DS

  %% DataStore to Services
  TW --> S_TW
  MA --> S_MA
  MI --> S_MI
  KB --> S_KB
```



## 2023年後半: TwitPane, ついぺんリサーチ, ZonePane
- Misskey対応

```mermaid
flowchart TD
  %% Android Apps subgraph
  subgraph AAP["🤖 Android Apps"]
    TP["📱 TwitPane"]
    TPR["📱 ついぺんリサーチ"]
    ZP["📱 ZonePane"]
  end

  %% UI Layer
  UI["🧑‍💻 UI (Activity / Fragment / Compose(一部))"]

  %% Logic Layer subgraph
  subgraph LG["🧠 Logic Layer"]
    VM["🧠 ViewModel(薄い)"]
    UC["🧭 UseCase"]
    RP["📦 Repository"]
  end

  %% DataSource subgraph
  subgraph DS["🗄️ DataStore"]
    TW["🐦 Twitter4J"]
    MA["🐘 mastodon4j"]
    MI["✨ **misskey4j**"]
    DB["💾 Local DB, Preferences"]
  end

  %% Service subgraph
  subgraph SV["🌍 Services"]
    S_TW["Twitter"]
    S_MA["Mastodon"]
    S_MI["Misskey"]
  end

  %% App connections
  TP --> UI
  TPR --> UI
  ZP --> UI

  %% UI to Logic
  UI --> VM --> UC --> RP

  %% Repository to DataSources
  RP --> DS

  %% DataStore to Services
  TW --> S_TW
  MA --> S_MA
  MI --> S_MI
```



## 2023年前半: TwitPane, ついぺんリサーチ, ZonePane
- ついぺんリサーチ
- ZonePane - Mastodon 対応

```mermaid
flowchart TD
  %% Android Apps subgraph
  subgraph AAP["🤖 Android Apps"]
    TP["📱 TwitPane"]
    TPR["📱 ついぺんリサーチ"]
    ZP["📱 **ZonePane**"]
  end

  %% UI Layer
  UI["🧑‍💻 UI (Activity / Fragment / Compose(一部))"]

  %% Logic Layer subgraph
  subgraph LG["🧠 Logic Layer"]
    VM["🧠 ViewModel(薄い)"]
    UC["🧭 UseCase"]
    RP["📦 Repository"]
  end

  %% DataSource subgraph
  subgraph DS["🗄️ DataStore"]
    TW["🐦 Twitter4J"]
    MA["🐘 **mastodon4j**"]
    DB["💾 Local DB, Preferences"]
  end

  %% Service subgraph
  subgraph SV["🌍 Services"]
    S_TW["Twitter"]
    S_MA["Mastodon"]
  end

  %% App connections
  TP --> UI
  TPR --> UI
  ZP --> UI

  %% UI to Logic
  UI --> VM --> UC --> RP

  %% Repository to DataSources
  RP --> DS

  %% DataStore to Services
  TW --> S_TW
  MA --> S_MA
```


## 2022年: TwitPane

```mermaid
flowchart TD
  %% Android Apps subgraph
  subgraph AAP["🤖 Android Apps"]
    APP["📱 TwitPane"]
  end

  %% UI Layer
  UI["🧑‍💻 UI (Activity / Fragment)"]

  %% Logic Layer subgraph
  subgraph LG["🧠 Logic Layer"]
    VM["🧠 ViewModel(薄い)"]
    UC["🧭 UseCase"]
    RP["📦 Repository"]
  end

  %% DataSource subgraph
  subgraph DS["🗄️ DataStore"]
    TW["🐦 Twitter4J"]
    DB["💾 Local DB, Preferences"]
  end

  %% Service subgraph
  subgraph SV["🌍 Services"]
    S_TW["Twitter"]
  end

  %% App connections
  APP --> UI

  %% UI to Logic
  UI --> VM --> UC --> RP

  %% Repository to DataSources
  RP --> DS

  %% DataStore to Services
  TW --> S_TW
```

