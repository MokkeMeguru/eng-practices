# Writing good CL descriptions

CL の description とは、**何を** 変更したのか **なぜ** 変更したのかについての公的な記録です。
プロダクトのバージョン管理の歴史の永続的な記録の一部となるものであり、何年にも渡ってその CL 以外のレビュワー以外にも何百人の人たちにも読まれる可能性があります。

将来の開発者は、記述した description を元に CL を検索することになります。
例えば将来誰かが、手元に詳細な情報がない状態でこの変更点を調べる際に、記述した description を参考にすることはあるでしょう。
重要な情報が説明文になく、コードとしてのみ存在している場合、この CL を探すのはとても骨の折れる作業になると考えられます。

Memo:

    CL にまつわる知識を引き継げるような description を書こう…？
    コード上に仕様を反映できないような実装ってあるんだろうか？ e.g. 特殊な背景による例外処理

## First Line {#firstline}

- 行われたことについての短い要約であること
- 命令形で書かれた完全な文であること
- 以降には空行をつけること

CL の最初の行は、**その CL によってなにが行われたのかの短い要約** である必要があります。
また、その後ろには必ず 1 行の空白を上げる必要があります。

ここでいう CL の最初の行とは、バージョンコントロールの履歴の要約部に表示されるものです。
将来コードを検索する人が、この CL やその説明文全体を読まなくても、この CL が何をしたのか、それまでと比較してどう変わったのかが理解できるような行でなければなりません。

つまり、1 行目は短く CL の内容のみに着目した要点のみを伝えるように気をつけてください。
ただし同時に読み手にとって価値のある情報を表現できているかを最優先に考えてください。

上述するような独立した最初の行を記述することで、読み手がコード履歴をより早く読み取ることができることが期待できます。

Memo:

    Github の PR で言えばタイトルですかね
    日本語の場合だと例えば、"課金システムとガチャシステムを処理する機能を分離する" みたいなのでも良さそう？
    この時、 "責務の分離とコードの凝集性を下げるために、" みたいな理由を書きたくなるけど、書いたほうが良いのか？ <=> 多分一覧性を考えると書かないほうが良さそう

## Body is Informative {#informative}

最初の行 [first line](#firstline) が短く、要約されるべきであるのに対して、残りの本文は、その CL に関わるすべての詳細を説明し、読み手が CL 全体を理解するためのいかなる追加情報も記述されるべきです。
言い換えれば、解決しようとしている問題と、その解法である CL の妥当性が読み手に理解できるように説明されているべきである、ということです。

仮にその CL で用いた手法に何らかの欠点や懸念事項があればそれは言及されるべきであり、必要に応じてバグのリンクやベンチマークの結果、そしてデザインや仕様書のリンクなどを記述するべきです。

もし外部リソースへのリンクを掲載するのであれば、そのリソースのアクセス制限や保存期間について考慮するとともに、できる限りレビュワーや将来の読者が理解できるような補足情報を記述する必要があります。

例えば小さな CL であっても詳細な内容について注意することは重要であり、CL が影響を及ぼす全体像を記述してください。

## Bad CL Descriptions {#bad}

"Fix bug" といった description は、不適切な CL の例です。
その CL が対応したのはどのようなバグなのか、そのバグをどのようなアプローチで修正したのか、といった情報が欠落しています。
他の似たような不適切な CL の例には次のようなものがあります。

- "Fix build."
- "Add patch."
- "Moving code from A to B."
- "Phase 1."
- "Add convenience functions."
- "kill weird URLs."
- "T/O"

これらの一部は Google 内部で実際に存在していた CL の説明です。
CL の作者は役に立つ情報を提供したと信じている可能性も否めませんが、実際には CL には十分な説明能力がありません。

## Good CL Descriptions {#good}

以下に有用な description の例を紹介します。

### Functionality change

Example:

> rpc: remove size limit on RPC server message freelist.
>
> Servers like FizzBuzz have very large messages and would benefit from reuse.
> Make the freelist larger, and add a goroutine that frees the freelist entries
> slowly over time, so that idle servers eventually release all freelist
> entries.

最初の行では、この CL が実際に行ったことを説明しています。
残りの本文では、解決した問題とその解決アプローチの妥当性を説明しており、更に特定の実装に対する追加情報が記述されています。

Memo:

    過去の PR からこれは良さそう、みたいなのをここの Example に導入したいところ

### Refactoring

Example:

> Construct a Task with a TimeKeeper to use its TimeStr and Now methods.
>
> Add a Now method to Task, so the borglet() getter method can be removed (which
> was only used by OOMCandidate to call borglet's Now method). This replaces the
> methods on Borglet that delegate to a TimeKeeper.
>
> Allowing Tasks to supply Now is a step toward eliminating the dependency on
> Borglet. Eventually, collaborators that depend on getting Now from the Task
> should be changed to use a TimeKeeper directly, but this has been an
> accommodation to refactoring in small steps.
>
> Continuing the long-range goal of refactoring the Borglet Hierarchy.

最初の行では、この CL が何を行い、過去のコードからどのように変更されたのかを示しています。
本文では、この CL によって、特定の実装について解決策が理想的でないこと、今後どのように改善していくのかを示しています。
更にこの変更を行った背景についても補足しています。

## Small CL that needs some context

Example:

> Create a Python3 build rule for status.py.
>
> This allows consumers who are already using this as in Python3 to depend on a
> rule that is next to the original status build rule instead of somewhere in
> their own tree. It encourages new consumers to use Python3 if they can,
> instead of Python2, and significantly simplifies some automated build file
> refactoring tools being worked on currently.

最初の文では、実際に行われたことを示しています。
残りの本文では、 **なぜ** その変更が行われたのかを説明し、レビュワーに多くの背景知識を提供することができています。

## Generated CL descriptions

一部の CL についてはツールによって生成されることがあります。
この際にも可能な限り上述したアドバイスに従うことをおすすめします。
つまり、最初の行において短く焦点が絞られた独立な文が記述される必要があります。
また、本文においてはレビュワーと将来のコード検索者が、各 CL の影響を理解するのを助ける有益な詳細情報を含める必要があります。

## Review the description before submitting the CL

CL は レビュー中にも大幅な変更が行われる可能性があります。
そのため、 CL をレビューに出す前に CL の description の内容が妥当かを見直す / レビューすることも有効です。

Next: [Small CLs](small-cls.md)
