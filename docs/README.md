# Google Engineering Practices Documentation

Google には、長年かつ広域な開発経験に基づいた、幅広い言語・プロジェクトを横断した技術的な知見が蓄積されています。
我々はこれらの知見について、オープンソースのプロジェクトや他の組織に対して利益をもたらすことができるのではないかと考え、できる限り公開することにしています。

現在我々は、以下のドキュメントを公開しています。

- [Google's Code Review Guidelines (コードレビューガイドライン)](review/index.md)

  さらに、コードレビューガイドは以下の 2 つのガイドラインに分けて考えることができます。

  - [The Code Reviewer's Guide (レビュワー向けのガイド)](review/reviewer/index.md)
  - [The Change Author's Guide (実装者向けのガイド)](review/developer/index.md)

## 用語

外部の読者に対して誤解を与えないよう、本ドキュメントにおいて利用される Google 内部で用いられる用語について補足します。

- **CL** "changelist"

  現在のコードレビューの対象である、変更差分のひとかたまりを指します。他の言い換えた表現としては "patch" や "change" というものがあります。

- **LGTM**: "Looks Good to me"

  ヨシ。コードレビュワーが CL に対して Approve する際に用いる。

Memo:

    CL > Github でいう PR というのが正しいかどうかは不明。例えば大規模な変更を小さな部分に分割した PR を出した際に、それは複数の CL と捉えるべきなのかどうか。あるいは Github Flow でいう Feature を指すのかもしれない
    LGTM > 変更差分についてヨシ。とするのか、変更についてヨシ。なのかは区別したほうが良いかも？ (いわゆるコード差分で LGTM を使えるのか、コード差分・検証(計画)・運用(計画)とかの話まで見た上で初めて LGTM を使えるのか、みたいなところ)

## License

The documents in this project are licensed under the
[CC-By 3.0 License](LICENSE), which encourages you to share these documents. See
<https://creativecommons.org/licenses/by/3.0/> for more details.

<a rel="license" href="https://creativecommons.org/licenses/by/3.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by/3.0/88x31.png" /></a>
