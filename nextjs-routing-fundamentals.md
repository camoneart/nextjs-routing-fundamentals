# ルーティングの基礎

**公開日:** 2025年10月31日

**原文:** https://nextjs.org/docs/14/app/building-your-application/routing

---

アプリケーションの骨格となるのはルーティングです。このページでは、Web向けのルーティングの**基本的な概念**と、Next.jsでルーティングを扱う方法を紹介します。

## 用語

まず、ドキュメント全体で使用される用語を確認しましょう。以下は簡単なリファレンスです：

![コンポーネントツリーの用語](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Fdocs%2Flight%2Fterminology-component-tree.png&w=3840&q=75)

- **Tree（ツリー）:** 階層構造を視覚化するための規則。例えば、親コンポーネントと子コンポーネントを持つコンポーネントツリー、フォルダ構造など。
- **Subtree（サブツリー）:** ツリーの一部で、新しいルート（最初）から始まり、リーフ（最後）で終わる。
- **Root（ルート）:** ツリーまたはサブツリーの最初のノード。例えばルートレイアウトなど。
- **Leaf（リーフ）:** サブツリー内で子を持たないノード。例えばURLパスの最後のセグメントなど。

![URLの構造の用語](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Fdocs%2Flight%2Fterminology-url-anatomy.png&w=3840&q=75)

- **URL Segment（URLセグメント）:** スラッシュで区切られたURLパスの一部。
- **URL Path（URLパス）:** ドメインの後に続くURLの部分（セグメントで構成される）。

## \`app\` Router

バージョン13で、Next.jsは[React Server Components](https://nextjs.org/docs/14/app/building-your-application/rendering/server-components)上に構築された新しい**App Router**を導入しました。これは、共有レイアウト、ネストされたルーティング、ローディング状態、エラーハンドリングなどをサポートしています。

App Routerは\`app\`という名前の新しいディレクトリで動作します。\`app\`ディレクトリは\`pages\`ディレクトリと並行して動作し、段階的な移行が可能です。これにより、アプリケーションの一部のルートを新しい動作にオプトインしながら、他のルートは以前の動作のために\`pages\`ディレクトリに保持できます。アプリケーションが\`pages\`ディレクトリを使用している場合は、[Pages Router](https://nextjs.org/docs/14/pages/building-your-application/routing)のドキュメントも参照してください。

> **Good to know（知っておくと良いこと）:** App RouterはPages Routerよりも優先されます。ディレクトリをまたぐルートは同じURLパスに解決されるべきではなく、競合を防ぐためにビルド時にエラーが発生します。

![Next.js App Directory](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Fdocs%2Flight%2Fnext-router-directories.png&w=3840&q=75)

デフォルトでは、\`app\`内のコンポーネントは[React Server Components](https://nextjs.org/docs/14/app/building-your-application/rendering/server-components)です。これはパフォーマンスの最適化であり、簡単に採用でき、[Client Components](https://nextjs.org/docs/14/app/building-your-application/rendering/client-components)も使用できます。

> **Recommendation（推奨）:** Server Componentsを初めて使う場合は、[Server](https://nextjs.org/docs/14/app/building-your-application/rendering/server-components)のページを確認してください。

## フォルダとファイルの役割

Next.jsはファイルシステムベースのルーターを使用します：

- **フォルダ**はルートを定義するために使用されます。ルートは、**ルートフォルダ**から\`page.js\`ファイルを含む最終的な**リーフフォルダ**まで、ファイルシステムの階層に従ったネストされたフォルダの単一パスです。[ルートの定義](https://nextjs.org/docs/14/app/building-your-application/routing/defining-routes)を参照してください。
- **ファイル**は、ルートセグメントに表示されるUIを作成するために使用されます。[特殊ファイル](https://nextjs.org/docs/14/app/building-your-application/routing#file-conventions)を参照してください。

## ルートセグメント

ルート内の各フォルダは**ルートセグメント**を表します。各ルートセグメントは、**URLパス**内の対応する**セグメント**にマッピングされます。

![ルートセグメントがURLセグメントにマッピングされる方法](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Fdocs%2Flight%2Froute-segments-to-path-segments.png&w=3840&q=75)

## ネストされたルート

ネストされたルートを作成するには、フォルダを互いにネストします。例えば、\`app\`ディレクトリに2つの新しいフォルダをネストすることで、新しい\`/dashboard/settings\`ルートを追加できます。

\`/dashboard/settings\`ルートは3つのセグメントで構成されます：

- \`/\`（ルートセグメント）
- \`dashboard\`（セグメント）
- \`settings\`（リーフセグメント）

## ファイル規約

Next.jsは、ネストされたルート内で特定の動作を持つUIを作成するための特殊ファイルのセットを提供します：

| | |
| --- | --- |
| [\`layout\`](https://nextjs.org/docs/14/app/building-your-application/routing/pages-and-layouts#layouts) | セグメントとその子のための共有UI |
| [\`page\`](https://nextjs.org/docs/14/app/building-your-application/routing/pages-and-layouts#pages) | ルートの固有のUIで、ルートを公開アクセス可能にする |
| [\`loading\`](https://nextjs.org/docs/14/app/building-your-application/routing/loading-ui-and-streaming) | セグメントとその子のためのローディングUI |
| [\`not-found\`](https://nextjs.org/docs/14/app/api-reference/file-conventions/not-found) | セグメントとその子のための404 UI |
| [\`error\`](https://nextjs.org/docs/14/app/building-your-application/routing/error-handling) | セグメントとその子のためのエラーUI |
| [\`global-error\`](https://nextjs.org/docs/14/app/building-your-application/routing/error-handling) | グローバルエラーUI |
| [\`route\`](https://nextjs.org/docs/14/app/building-your-application/routing/route-handlers) | サーバーサイドAPIエンドポイント |
| [\`template\`](https://nextjs.org/docs/14/app/building-your-application/routing/pages-and-layouts#templates) | 特殊な再レンダリングされるレイアウトUI |
| [\`default\`](https://nextjs.org/docs/14/app/api-reference/file-conventions/default) | [Parallel Routes](https://nextjs.org/docs/14/app/building-your-application/routing/parallel-routes)のためのフォールバックUI |

> **Good to know（知っておくと良いこと）:** \`.js\`、\`.jsx\`、または\`.tsx\`のファイル拡張子を特殊ファイルに使用できます。

## コンポーネント階層

ルートセグメントの特殊ファイルで定義されたReactコンポーネントは、特定の階層でレンダリングされます：

- \`layout.js\`
- \`template.js\`
- \`error.js\`（React error boundary）
- \`loading.js\`（React suspense boundary）
- \`not-found.js\`（React error boundary）
- \`page.js\`またはネストされた\`layout.js\`

![ファイル規約のコンポーネント階層](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Fdocs%2Flight%2Ffile-conventions-component-hierarchy.png&w=3840&q=75)

ネストされたルートでは、セグメントのコンポーネントは親セグメントのコンポーネントの**内側**にネストされます。

![ネストされたファイル規約のコンポーネント階層](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Fdocs%2Flight%2Fnested-file-conventions-component-hierarchy.png&w=3840&q=75)

## コロケーション

特殊ファイルに加えて、\`app\`ディレクトリ内のフォルダに独自のファイル（コンポーネント、スタイル、テストなど）を配置するオプションがあります。

これは、フォルダがルートを定義する一方で、\`page.js\`または\`route.js\`によって返されるコンテンツのみが公開アドレス可能であるためです。

![コロケーションされたファイルを含むフォルダ構造の例](https://nextjs.org/_next/image?url=https%3A%2F%2Fh8DxKfmAPhn8O0p3.public.blob.vercel-storage.com%2Fdocs%2Flight%2Fproject-organization-colocation.png&w=3840&q=75)

[プロジェクト構成とコロケーション](https://nextjs.org/docs/14/app/building-your-application/routing/colocation)について詳しく学ぶ。

## 高度なルーティングパターン

App Routerは、より高度なルーティングパターンを実装するのに役立つ一連の規約も提供します。これらには以下が含まれます：

- [Parallel Routes](https://nextjs.org/docs/14/app/building-your-application/routing/parallel-routes): 同じビューで2つ以上のページを同時に表示でき、それぞれ独立してナビゲートできます。独自のサブナビゲーションを持つ分割ビューに使用できます。例：ダッシュボード。
- [Intercepting Routes](https://nextjs.org/docs/14/app/building-your-application/routing/intercepting-routes): ルートをインターセプトし、別のルートのコンテキスト内で表示できます。現在のページのコンテキストを保持することが重要な場合に使用できます。例：1つのタスクを編集しながらすべてのタスクを表示する、またはフィード内の写真を拡大する。

これらのパターンにより、より豊かで複雑なUIを構築でき、歴史的に小規模なチームや個々の開発者が実装するのが複雑だった機能を民主化できます。

## 次のステップ

Next.jsでのルーティングの基礎を理解したので、以下のリンクに従って最初のルートを作成してください：

- [**ルートの定義** - Next.jsで最初のルートを作成する方法を学ぶ](https://nextjs.org/docs/14/app/building-your-application/routing/defining-routes)
- [**ページとレイアウト** - App Routerで最初のページと共有レイアウトを作成する](https://nextjs.org/docs/14/app/building-your-application/routing/pages-and-layouts)
- [**リンクとナビゲーション** - Next.jsでのナビゲーションの仕組みと、Linkコンポーネントと\`useRouter\`フックの使い方を学ぶ](https://nextjs.org/docs/14/app/building-your-application/routing/linking-and-navigating)
- [**ローディングUIとストリーミング** - Suspenseの上に構築されたLoading UIにより、特定のルートセグメントのフォールバックを作成し、準備ができたコンテンツを自動的にストリーミングできる](https://nextjs.org/docs/14/app/building-your-application/routing/loading-ui-and-streaming)
- [**エラーハンドリング** - ルートセグメントとそのネストされた子をReact Error Boundaryで自動的にラップすることで、ランタイムエラーを処理する](https://nextjs.org/docs/14/app/building-your-application/routing/error-handling)
- [**リダイレクト** - Next.jsでリダイレクトを処理するさまざまな方法を学ぶ](https://nextjs.org/docs/14/app/building-your-application/routing/redirecting)
- [**ルートグループ** - ルートグループを使用してNext.jsアプリケーションを異なるセクションに分割できる](https://nextjs.org/docs/14/app/building-your-application/routing/route-groups)
- [**プロジェクト構成** - Next.jsプロジェクトの構成方法とファイルの配置方法を学ぶ](https://nextjs.org/docs/14/app/building-your-application/routing/colocation)
- [**動的ルート** - 動的ルートを使用して、動的データからルートセグメントをプログラム的に生成できる](https://nextjs.org/docs/14/app/building-your-application/routing/dynamic-routes)
- [**Parallel Routes** - 同じビューで1つ以上のページを同時にレンダリングし、それぞれ独立してナビゲートできる。非常に動的なアプリケーション向けのパターン](https://nextjs.org/docs/14/app/building-your-application/routing/parallel-routes)
- [**Intercepting Routes** - 現在のレイアウト内で新しいルートを読み込みながらブラウザのURLをマスクするインターセプトルートを使用する。モーダルなどの高度なルーティングパターンに便利](https://nextjs.org/docs/14/app/building-your-application/routing/intercepting-routes)
- [**ルートハンドラー** - WebのRequestとResponse APIを使用して、特定のルートのカスタムリクエストハンドラーを作成する](https://nextjs.org/docs/14/app/building-your-application/routing/route-handlers)
- [**ミドルウェア** - リクエストが完了する前にコードを実行するミドルウェアの使い方を学ぶ](https://nextjs.org/docs/14/app/building-your-application/routing/middleware)
- [**国際化** - 国際化されたルーティングとローカライズされたコンテンツで複数の言語をサポートする](https://nextjs.org/docs/14/app/building-your-application/routing/internationalization)
