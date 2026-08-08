系統樹 v43 — クリック・lazy load修正

構成
index.html
data/
  manifest.json
  core.json
  animalia/
    aves.json
    aves.index.json
  plantae/
    embryophyta.json
    embryophyta.index.json

ローカルで試す
1. ZIPを展開します。
2. index.html をブラウザで開きます。
3. 「データフォルダ選択」で、このパッケージの data フォルダを選びます。
4. 植物界 → 陸上植物 をクリックすると embryophyta.json をその時点で読み込みます。
5. 動物界 → 脊索動物門 → 鳥綱をクリックすると aves.json を読み込みます。
6. 検索時は各 *.index.json のみ先に読み、未読込の分類群を選んだ時に本体JSONをロードします。

HTTP / GitHub Pages
manifest.json と core.json は起動時に自動取得します。
各 branch JSON は必要になるまで取得しません。

植物データ v1
- Embryophyta（陸上植物）
- 主要クレード + 目 + 科
- 914 nodes
- 159 orders
- 712 families
- APG IV: 被子植物 64目416科
- PPG I: 小葉植物・シダ植物 14目51科
- Yang et al. 2022: 裸子植物 8目13科
- BPG 2024: コケ植物 73目232科


鳥類軽量化
- 種ノードを 11,131 件削除
- 鳥類本体: 13,865 nodes → 2,734 nodes
- クレード・目・科・属は維持
- 属ノードを末端として表示
- 検索indexからも種を削除


鳥類さらに軽量化
- 属ノードを 2,376 件削除
- 鳥類本体: 2,734 nodes → 358 nodes
- 高次クレード・目・科を維持
- 科ノードを末端として表示
- 検索indexからも属を削除


哺乳類データ v1
- 基準: ASM Mammal Diversity Database v2.5 (2026-07-28)
- 高次系統: conservative mammal phylogeny backbone
- 27 orders
- 169 families
- 44 higher clade/rank nodes
- 241 nodes total
- 日本語: 目 27/27、科 141/169（確信のあるもの中心）
- MDD同様、約1500年以降に絶滅した分類群も含む


検索改善
- Unicode NFKC 正規化
- カタカナをひらがなへ正規化して比較
- 「カエル」と「かえる」を同一視
- 半角カナも同一視

両生類データ v1
- 基準: Amphibian Species of the World 6.2
- 3目・79科
- ASWで明示される3上科を挿入
- 86 nodes total
- 日本語: 目 3/3、科 29/79


竜弓類 / 爬虫類データ v1
- root: Sauropsida
- 現生の非鳥類爬虫類: 4目・93科
- Testudines: 14科
- Rhynchocephalia: 1科
- Squamata: 75科
- Crocodylia: 3科
- 鳥類 Aves は Archosauria の子として既存 bird branch を遅延ロード
- Sauropsida → Lepidosauria / Archelosauria
- Archelosauria → Testudines / Archosauria
- Archosauria → Crocodylia / Aves
- Aves branch は dependsOn: sauropsida により、検索から直接鳥へ飛んでも親系統を先にロード


v21 配置比較
- 「円形」: 従来の円形・局所ファン/全周ハイブリッド
- 「横一列」: 同じ深さの兄弟を1本の横列へ配置
- 選択中の子は親の真下へ揃える
- 新しく開いた階層は親を中心として左右へ展開
- 兄弟数が多いと横方向に非常に広くなるため、ドラッグ・ホイールで確認
- 配置切替はデータ・検索・比較タブには影響しない


v22 横一列ビュー改善
- 横一列モードでは、現在選択しているノードを常に表示領域の中央へ配置
- 兄弟数が多くても「全ノードを画面内へ縮小」しない
- 横一列の基準viewportは 1120 x 700 world units
- クリックするたびに選択ノード中心へ再センタリング
- その後のホイールズーム・ドラッグパンは従来通り可能
- 円形モードのfit挙動は変更なし


v23 魚類
- 横一列ビューをデフォルト化（円形切替は維持）
- FishBase current classification page: 93 order-level rows / 626 families
- 今回は科まで入れず、目レベルで停止
- FishBase の 3つの /misc bucket は clade として保持
- FishBase の Perciformes/xxx 10行は 1つの Perciformes の下の suborder として正規化
- formal order nodes: 81
- 日本語目名: 69/81
- 「Pisces」ノードは作らない
- Vertebrata
  - Cyclostomata
  - Gnathostomata
    - Chondrichthyes
    - Osteichthyes
      - Actinopterygii
      - Sarcopterygii
        - Coelacanthiformes
        - Dipnoi
        - Tetrapoda
          - Amphibia
          - Mammalia
          - Sauropsida
- Mammalia / Amphibia / Sauropsida は vertebrata を dependency として遅延ロード
- Aves は従来通り Sauropsida -> Archosauria 内で遅延ロード


v24 表示調整
- 省略（… N階層）を作る際、現在選択しているノードの直上の親ノードは必ず実名表示
- 選択ノード自身は従来通り必ず表示
- 主要階級（界・門・綱・目・科・属）も従来通り省略しない
- 円形モードの選択経路方向を上（-90°）から下（+90°）へ変更
- 横一列モードも正のY方向へ伸びるため、両レイアウトの進行方向を下向きに統一


v25 昆虫
- Insecta を Arthropoda の子として追加
- 現生28目
- 日本語目名 28/28
- Isoptera（シロアリ目）は独立させず Blattodea（ゴキブリ目）に含める
- Phthiraptera（シラミ類）は独立させず Psocodea（カジリムシ目）に含める
- Insecta
  - Archaeognatha
  - Dicondylia
    - Zygentoma
    - Pterygota
      - Ephemeroptera
      - Odonata
      - Neoptera
        - Polyneoptera
        - Psocodea
        - Thysanoptera
        - Hemiptera
        - Holometabola
- Polyneoptera内部は議論が残るため、Dictyoptera と Xenonomia 以外を過度に解決しない
- Holometabola は Hymenoptera + Aparaglossata を軸に主要クレードを挿入


v26 無脊椎動物の追加
Animalia
- Cnidaria
- Bilateria
  - Protostomia
    - Arthropoda
    - Mollusca
  - Deuterostomia
    - Echinodermata
    - Chordata

注意:
- Protostomia / Deuterostomia の子は「現在このアプリに実装した門」だけで、全門を列挙したものではない。

Mollusca
- Aculifera
  - Aplacophora
    - Caudofoveata
    - Solenogastres
  - Polyplacophora
- Conchifera
  - Monoplacophora
  - Cephalopoda
  - Gastropoda + Diasoma
    - Gastropoda
    - Diasoma
      - Scaphopoda
      - Bivalvia

Echinodermata
- Crinozoa -> Crinoidea
- Eleutherozoa
  - Asterozoa -> Asteroidea / Ophiuroidea
  - Echinozoa -> Echinoidea / Holothuroidea

Cnidaria
- Anthozoa -> Hexacorallia / Octocorallia
- Medusozoa -> Hydrozoa / Scyphozoa / Cubozoa / Staurozoa
- Myxozoa -> Myxosporea / Malacosporea


v27 動物界の追加
Animalia
- Porifera
- Ctenophora
- Cnidaria
- Bilateria
  - Protostomia
    - Spiralia
      - Platyhelminthes
      - Mollusca
      - Annelida
    - Ecdysozoa
      - Nematoda
      - Arthropoda
  - Deuterostomia
    - Ambulacraria
      - Echinodermata
      - Hemichordata
    - Chordata

追加粒度:
- Porifera: 4現生綱
- Ctenophora: 2クラス（深い内部系統は解決しすぎない）
- Platyhelminthes: Catenulida / Rhabditophora + 主要自由生活目 + Neodermata
- Annelida: Palaeoannelida / Pleistoannelida -> Errantia / Sedentaria + Clitellata
- Nematoda: Enoplea / Chromadorea（旧 Adenophorea / Secernentea は使わない）
- Hemichordata: Enteropneusta / Graptolithoidea


v28 節足動物
Arthropoda
- Chelicerata
  - Pycnogonida
  - Xiphosura
  - Arachnida
    - familiar arachnid orders
    - Acari -> Acariformes / Parasitiformes
- Mandibulata
  - Myriapoda
    - Chilopoda / Diplopoda / Symphyla / Pauropoda
  - Pancrustacea
    - Oligostraca
    - Multicrustacea
      - Malacostraca（Decapoda, Isopoda, Amphipoda, Stomatopoda, Euphausiacea, Mysida）
      - Copepoda
      - Thecostraca
    - Allotriocarida
      - Branchiopoda
      - Cephalocarida
      - Remipedia
      - Hexapoda
        - Collembola / Protura / Diplura / Insecta

重要:
- Crustacea を単系統ノードとしては作らない。
- Hexapoda / Insecta は Pancrustacea 内へ移動。
- Insecta branch は dependsOn: pancrustacea で親系統を先にロード。

UI:
- ヘッダーに「主要グループへ…」select を追加。
- 動物界、節足動物、鋏角類、多足類、汎甲殻類、昆虫、脊椎動物、鳥、哺乳類、両生類、
  竜弓類、軟体、棘皮、刺胞、陸上植物へ検索入力なしで直接ジャンプ。


v29 LIFE / 真核生物
LIFE
- Bacteria 真正細菌
- Archaea 古細菌
  - Asgardarchaeota アスガルド古細菌
- Eukaryota 真核生物
  - Metamonada メタモナダ
  - Amorphea
    - Amoebozoa
      - Tubulinea
      - Discosea
      - Evosea
    - Obazoa
      - Opisthokonta
        - Holomycota -> Fungi
        - Holozoa
          - Choanoflagellatea 襟鞭毛虫類
          - Animalia
  - Diaphoretickes
    - Archaeplastida -> Plantae
    - SAR
      - Stramenopiles
        - Ochrophyta -> Bacillariophyta / Phaeophyceae
        - Oomycota
      - Alveolata
        - Ciliophora
        - Dinoflagellata
        - Apicomplexa
      - Rhizaria
        - Cercozoa
        - Foraminifera
        - Radiolaria

注意:
- LIFE直下はナビゲーション用の operational 3-way display。
- 現在の二ドメイン系統では、真核生物の祖先は古細菌、特にAsgard系統と非常に近い。
- 真核生物の深い根は2025年の系統ゲノム研究でも更新が続いているため、
  v29は要求された主要群を安全に配置する部分骨格。
- domain rank を KEEP_RANKS に追加し、真正細菌・古細菌・真核生物は「…」で省略しない。
- 主要グループジャンプにBacteria / Archaea / Asgard / Eukaryota / Metamonada /
  Amoebozoa / Choanoflagellatea / Stramenopiles / Alveolata / SARを追加。


v30 菌界
- 2024 Fungal Diversity の19門を採用
- Rozellomycota / Aphelidiomycota
- Chytridiomyceta: Chytridiomycota / Monoblepharomycota / Neocallimastigomycota
- Blastocladiomycota + Sanchytriomycota
- Olpidiomycota
- Zoopagomyceta: Basidiobolomycota / Entomophthoromycota / Kickxellomycota / Zoopagomycota
- Mucoromyceta: Calcarisporiellomycota / Glomeromycota / Mortierellomycota / Mucoromycota
- Dikarya: Entorrhizomycota / Ascomycota / Basidiomycota

v30 穴埋め
- Eukaryota: Discoba -> Jakobida / Heterolobosea / Euglenozoa
- Diaphoretickes: Cryptista / Haptista を追加
- Archaeplastida: Glaucophyta / Rhodophyta / Chloroplastida
- 緑色植物: Chlorophyta / Streptophyta -> Embryophyta
- Bilateria: Xenacoelomorpha を独立表示
- Spiralia: Rotifera / Nemertea
- Ecdysozoa: Tardigrada / Onychophora


v31 代表種
- 勉強用に一部分類群へ representative species ノードを追加
- rank: species
- 表示: 白丸 + 濃い縁
- 和名 / 英名 / 学名 / Wikipediaリンクつき
- 追加先:
  - Aves (families)
  - Mammalia
  - Amphibia
  - Sauropsida
  - Insecta
  - Vertebrata (fish orders)
  - Embryophyta
  - Fungi
- 代表種数: 55


v32
- species ノードに専用クラス .node.species を追加
- 白丸のふちが白に埋もれないよう、濃いストロークで表示


v33
- 代表種の親ノードを、可能なものは「科」に移動
  - Amphibia / Sauropsida / Embryophyta を order 親から family 親へ再配置
  - Aves / Mammalia は family 親のまま維持
  - Insecta / fish / Fungi は現行粒度のまま
- UI追加:
  - order などを選んだとき、表示中の family row の下に representative species をプレビュー表示
  - 実際のデータ構造は family -> species を維持


v34
- ヘッダーに「代表種: ON/OFF」ボタンを追加
- OFF のとき:
  - species ノード本体を非表示
  - family row 下の representative species プレビューも非表示
- ON のとき:
  - 従来どおり表示


v35 代表種の外部JSON化
- taxonomy branch JSON から rank=species の代表種を除去
- data/representative_species.json を唯一の代表種ソースに変更
- 現在 194 種
- 内訳: {'amphibia': 21, 'aves': 50, 'embryophyta': 52, 'fungi': 5, 'insecta': 8, 'mammalia': 31, 'sauropsida': 20, 'vertebrata': 7}
- representative_species.json の1エントリ:
  id / ja / en / latin / parent / branch / priority / wiki / color
- branchをロードした時点で、parent family が存在する代表種だけruntime overlayとして注入
- 検索indexにも代表種を動的に追加
- ON/OFF機能は維持
- 今後はtaxonomy JSONを触らず representative_species.json に追加するだけ


v36 代表種表示
- species circle radius: 28 -> 18
- species label: 11.5px -> 8.5px
- species rank: 8.5px -> 6.5px
- family -> representative species preview line: 3px active line -> 0.8px thin line
- preview spacingも少し詰めた
- 通常の分類ノードのサイズ・線幅は変更なし


v37
- family -> species のプレビューは従来どおり
- order -> species の直結データだけ追加ルール:
  - order の immediate parent（clade / class 等）を選択した時点で species を先行表示
  - deeper descendant の species は再帰的に引き上げない
- species に接続する枝はすべて細い濃色の破線
  - stroke: #222b31
  - width: 0.85
  - dash: 4 3


v38 representative species
- 194 -> 304 species (+110)
- additions:
  - Insecta +40
  - fish/Vertebrata +30
  - Fungi +15
  - Embryophyta +25
- priority reorganization:
  - priority 1: 152 core study species
  - priority 2: 152 extended species
- UI:
  - [代表種: 重要] = priority 1 only
  - [全部] = priority 1 + 2
  - [OFF] = hide representative species
- Search still contains all 304 species.
  Selecting a hidden priority-2 species from search automatically switches to 全部.
- counts by branch:
{
  "amphibia": {
    "total": 21,
    "priority1": 15,
    "priority2": 6
  },
  "aves": {
    "total": 50,
    "priority1": 30,
    "priority2": 20
  },
  "embryophyta": {
    "total": 77,
    "priority1": 30,
    "priority2": 47
  },
  "fungi": {
    "total": 20,
    "priority1": 8,
    "priority2": 12
  },
  "insecta": {
    "total": 48,
    "priority1": 20,
    "priority2": 28
  },
  "mammalia": {
    "total": 31,
    "priority1": 20,
    "priority2": 11
  },
  "sauropsida": {
    "total": 20,
    "priority1": 14,
    "priority2": 6
  },
  "vertebrata": {
    "total": 37,
    "priority1": 15,
    "priority2": 22
  }
}


v39 coverage-oriented representative species
- total: 304 -> 354 (+50)
- additions:
  - Insecta +14
  - fish/Vertebrata +16
  - Embryophyta +20
- rule:
  - do not add to orders already carrying >=5 representatives
  - prioritize zero-covered orders
  - priority 1 is max one per order where an order layer exists
  - additions are mainly priority 2
- priority 1 -> priority 2 demotions: 82
- current priority:
  - priority 1: 87
  - priority 2: 267
- priority 2 styling:
  - slightly gray fill #eef1f3
  - slightly lighter/thinner outline and text opacity
- coverage:
{
  "insecta": {
    "orders": 28,
    "covered_orders": 24,
    "zero_orders": 4,
    "orders_5plus": 5
  },
  "vertebrata": {
    "orders": 81,
    "covered_orders": 34,
    "zero_orders": 47,
    "orders_5plus": 1
  },
  "embryophyta": {
    "orders": 159,
    "covered_orders": 47,
    "zero_orders": 112,
    "orders_5plus": 6
  },
  "aves": {
    "orders": 46,
    "covered_orders": 14,
    "zero_orders": 32,
    "orders_5plus": 2
  },
  "mammalia": {
    "orders": 27,
    "covered_orders": 8,
    "zero_orders": 19,
    "orders_5plus": 3
  },
  "amphibia": {
    "orders": 3,
    "covered_orders": 2,
    "zero_orders": 1,
    "orders_5plus": 2
  },
  "sauropsida": {
    "orders": 4,
    "covered_orders": 2,
    "zero_orders": 2,
    "orders_5plus": 2
  }
}


v40
- Bryophytes study-view simplification:
  - removed 232 family nodes below Bryophytes
  - reparented 3 existing representative species from family -> order
  - plant nodes: 682
  - plant families: 480
  - bryophyte families shown: 0
- representative species:
  - 354 -> 419 (+65)
  - all v40 additions have japan_status=natural
  - priority1=111, priority2=308
- v40 additions focus:
  - Fungi +10
  - Mollusca +16
  - Cnidaria +8
  - Echinodermata +8
  - Porifera +2
  - Ctenophora +2
  - Annelida +2
  - Hemichordata +1
  - Chelicerata +5
  - Myriapoda +2
  - Pancrustacea +8
  - hornwort +1
- direct representative preview generalized:
  - if any displayed taxonomy node directly owns species, its species may preview one level early
  - useful for class-level/phylum-level low-resolution branches


v41 smartphone
- touch gestures:
  - one finger drag = pan
  - two finger pinch = zoom
  - two finger midpoint motion = pan while pinching
  - desktop wheel zoom remains
- all_in_one.json:
  - 2281 nodes
  - merges core + all lazy branches + 419 representative species
  - intended for smartphone/offline single-JSON import
- standalone quick-jump fixed:
  - if the target node is already loaded, navigate directly without attempting a manifest branch load

Recommended mobile loading:
1. Best: publish the whole package with GitHub Pages; JSON loads automatically.
2. Offline/local: unzip, open index.html, tap "単一JSON読込（スマホ向け）", choose all_in_one.json.


v42 default display
- fitted view starts at 2x zoom, centered on the same focus point
- representative species starts in 全部 mode
- pinch / one-finger pan / desktop wheel behavior unchanged


v43 bug fixes
1. Mouse/touch click:
   - no pointer capture on pointerdown
   - pointer capture starts only after single-pointer drag threshold (>5px)
   - pinch captures pointers only after the second pointer appears
   - normal click/tap therefore reaches each SVG <g> node again

2. Lazy branch roots after quick jump:
   - removed the incorrect nodes.has(id) == loaded test
   - explicit branchKey is used only when that branch exists in the current manifest
   - split-data mode can load a branch even when its root placeholder already exists
   - all_in_one standalone mode still ignores static quick-jump branch keys safely
