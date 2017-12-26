# Rust API Guidelines Checklist

<!-- Read CONTRIBUTING.md before writing new guidelines -->

- **命名** *(クレートがRustの慣用的な命名規則に従っている)*
  - [ ] 大文字・小文字の使い分けがRFC430に従っている ([C-CASE])
  - [ ] 変換メソッドに`as_`, `to_`, `into_`を使っている ([C-CONV])
  - [ ] Getterの名前がRustの規則に従っている ([C-GETTER])
  - [ ] イテレータを生成するメソッドの名前が`iter`, `iter_mut`, `into_iter`となっている ([C-ITER])
  - [ ] イテレータの型名が、それを生成するメソッドと揃っている ([C-ITER-TY])
  - [ ] Featureの名前に余計な単語が入っていない ([C-FEATURE])
  - [ ] 命名時に単語を並べる順番が揃っている ([C-WORD-ORDER])
- **相互運用性** *(クレートが他のクレートの機能とうまく連携できる)*
  - [ ] 積極的に一般的なトレイトを型に実装している ([C-COMMON-TRAITS])
    - `Copy`, `Clone`, `Eq`, `PartialEq`, `Ord`, `PartialOrd`, `Hash`, `Debug`,
      `Display`, `Default`
  - [ ] 変換に標準のトレイト`From`, `AsRef`, `AsMut`を用いている ([C-CONV-TRAITS])
  - [ ] コレクションが`FromIterator`と`Extend`を実装している ([C-COLLECT])
  - [ ] データ構造がSerdeの`Serialize`と`Deserialize`を実装している ([C-SERDE])
  - [ ] 型が可能な限り`Send`,`Sync`である ([C-SEND-SYNC])
  - [ ] エラー型の意味が分かりやすく、行儀の良い実装となっている ([C-GOOD-ERR])
  - [ ] バイナリ数値型が`Hex`, `Octal`, `Binary`によるフォーマットをサポートしている ([C-NUM-FMT])
  - [ ] 読み書きを行うジェネリックな関数が`R: Read`と`W: Write`を値渡しで受け取っている ([C-RW-VALUE])
- **マクロ** *(クレートが行儀のよいマクロを提供している)*
  - [ ] 入力の構文から結果をイメージできるようになっている ([C-EVOCATIVE])
  - [ ] アイテムを宣言するマクロが属性と衝突しない ([C-MACRO-ATTR])
  - [ ] アイテムを宣言するマクロがアイテムを宣言できる場所のどこでも使える ([C-ANYWHERE])
  - [ ] アイテムを宣言するマクロが可視性の指定をサポートしている ([C-MACRO-VIS])
  - [ ] 型の指定が柔軟である ([C-MACRO-TY])
- **ドキュメンテーション** *(クレートに十分なドキュメントが付けられている)*
  - [ ] クレートレベルにコード例付きの詳細なドキュメントがある ([C-CRATE-DOC])
  - [ ] 全てのアイテムにコード例が付いている ([C-EXAMPLE])
  - [ ] コード例が`try!`や`unwrap`ではなく`?`を使っている ([C-QUESTION-MARK])
  - [ ] 関数のドキュメントにエラー、パニック、安全性に関する事項が含まれている ([C-FAILURE])
  - [ ] 文章に関係する項目へのリンクを含める ([C-LINK])
  - [ ] Cargo.tomlが一般的なメタデータを全て含んでいる ([C-METADATA])
    - authors, description, license, homepage, documentation, repository,
      readme, keywords, categories
  - [ ] 大きな変更が全てリリースノートに記載されている ([C-RELNOTES])
  - [ ] 無用な実装詳細がRustdocに表示されていない ([C-HIDDEN])
- **予測性** *(クレートを使い、見かけ通りに動作する読みやすいコードが書ける)*
  - [ ] スマートポインタがinherentメソッドを持っていない ([C-SMART-PTR])
  - [ ] 変換メソッドは最も関係の深い型に付ける ([C-CONV-SPECIFIC])
  - [ ] 明確なレシーバを持つ関数はメソッドにする ([C-METHOD])
  - [ ] 関数がoutパラメータを持たない ([C-NO-OUT])
  - [ ] 奇妙な演算子オーバーロードを行っていない ([C-OVERLOAD])
  - [ ] `Deref`と`DerefMut`を実装しているのはスマートポインタだけである ([C-DEREF])
  - [ ] コンストラクタはスタティックなinherentメソッドである ([C-CTOR])
- **柔軟性** *(クレートが実用的なユースケースを幅広くカバーしている)*
  - [ ] 重複した処理を行わなくて済むように中間生成物を公開している ([C-INTERMEDIATE])
  - [ ] 呼び出し側がデータをコピーするタイミングを決める ([C-CALLER-CONTROL])
  - [ ] ジェネリクスを用いて関数の引数に対する制限を最小にしている ([C-GENERIC])
  - [ ] トレイトオブジェクトとして有用なトレイトはオブジェクトセーフになっている ([C-OBJECT])
- **型安全性** *(クレートが型システムを有効に活用している)*
  - [ ] newtypeを使って静的に値を区別する ([C-NEWTYPE])
  - [ ] `bool`や`Option`の代わりに意味のある型を使う ([C-CUSTOM-TYPE])
  - [ ] フラグの集合は、列挙型ではなく`bitflags`で表す ([C-BITFLAG])
  - [ ] 複雑な値の生成にはビルダーパターンを使う ([C-BUILDER])
- **信頼性** *(クレートが間違ったことをしない)*
  - [ ] 関数が引数を検証している ([C-VALIDATE])
  - [ ] デストラクタが失敗しない ([C-DTOR-FAIL])
  - [ ] ブロックする可能性のあるデストラクタには代替手段を用意する ([C-DTOR-BLOCK])
- **デバッガビリティ** *(クレートが容易なデバッグを支援している)*
  - [ ] 全てのパブリックな型に`Debug`を実装する ([C-DEBUG])
  - [ ] `Debug`表現を空にしない ([C-DEBUG-NONEMPTY])
- **将来性** *(クレートを、ユーザのコードを壊すことなく改善できる)*
  - [ ] sealedトレイトを使って下流の実装を適切に防いでいる ([C-SEALED])
  - [ ] 構造体のフィールドを適切にプライベートにする ([C-STRUCT-PRIVATE])
  - [ ] newtypeを用いて実装詳細を隠蔽している ([C-NEWTYPE-HIDE])
  - [ ] データ構造にderiveしたトレイトの境界を定義で繰り返さない ([C-STRUCT-BOUNDS])
- **必要事項** *(ある状況下では重要な問題)*
  - [ ] stableなクレートのパブリックな依存クレートがstableである ([C-STABLE])
  - [ ] クレートとその依存先がpermissiveなライセンスの下にある ([C-PERMISSIVE])


[C-CASE]: naming.html#c-case
[C-CONV]: naming.html#c-conv
[C-GETTER]: naming.html#c-getter
[C-ITER]: naming.html#c-iter
[C-ITER-TY]: naming.html#c-iter-ty
[C-FEATURE]: naming.html#c-feature
[C-WORD-ORDER]: naming.html#c-word-order

[C-COMMON-TRAITS]: interoperability.html#c-common-traits
[C-CONV-TRAITS]: interoperability.html#c-conv-traits
[C-COLLECT]: interoperability.html#c-collect
[C-SERDE]: interoperability.html#c-serde
[C-SEND-SYNC]: interoperability.html#c-send-sync
[C-GOOD-ERR]: interoperability.html#c-good-err
[C-NUM-FMT]: interoperability.html#c-num-fmt
[C-RW-VALUE]: interoperability.html#c-rw-value

[C-EVOCATIVE]: macros.html#c-evocative
[C-MACRO-ATTR]: macros.html#c-macro-attr
[C-ANYWHERE]: macros.html#c-anywhere
[C-MACRO-VIS]: macros.html#c-macro-vis
[C-MACRO-TY]: macros.html#c-macro-ty

[C-CRATE-DOC]: documentation.html#c-crate-doc
[C-EXAMPLE]: documentation.html#c-example
[C-QUESTION-MARK]: documentation.html#c-question-mark
[C-FAILURE]: documentation.html#c-failure
[C-LINK]: documentation.html#c-link
[C-METADATA]: documentation.html#c-metadata
[C-HTML-ROOT]: documentation.html#c-html-root
[C-RELNOTES]: documentation.html#c-relnotes
[C-HIDDEN]: documentation.html#c-hidden

[C-SMART-PTR]: predictability.html#c-smart-ptr
[C-CONV-SPECIFIC]: predictability.html#c-conv-specific
[C-METHOD]: predictability.html#c-method
[C-NO-OUT]: predictability.html#c-no-out
[C-OVERLOAD]: predictability.html#c-overload
[C-DEREF]: predictability.html#c-deref
[C-CTOR]: predictability.html#c-ctor

[C-INTERMEDIATE]: flexibility.html#c-intermediate
[C-CALLER-CONTROL]: flexibility.html#c-caller-control
[C-GENERIC]: flexibility.html#c-generic
[C-OBJECT]: flexibility.html#c-object

[C-NEWTYPE]: type-safety.html#c-newtype
[C-CUSTOM-TYPE]: type-safety.html#c-custom-type
[C-BITFLAG]: type-safety.html#c-bitflag
[C-BUILDER]: type-safety.html#c-builder

[C-VALIDATE]: dependability.html#c-validate
[C-DTOR-FAIL]: dependability.html#c-dtor-fail
[C-DTOR-BLOCK]: dependability.html#c-dtor-block

[C-DEBUG]: debuggability.html#c-debug
[C-DEBUG-NONEMPTY]: debuggability.html#c-debug-nonempty

[C-SEALED]: future-proofing.html#c-sealed
[C-STRUCT-PRIVATE]: future-proofing.html#c-struct-private
[C-NEWTYPE-HIDE]: future-proofing.html#c-newtype-hide
[C-STRUCT-BOUNDS]: future-proofing.html#c-struct-bounds

[C-STABLE]: necessities.html#c-stable
[C-PERMISSIVE]: necessities.html#c-permissive
