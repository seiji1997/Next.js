
# Next.js 問題・回答・解説

## 問題

以下の指示に従って、`pages/index.tsx`ファイルを編集してください。

1. タイトル「トップページ」をブラウザのタブに表示させてください。(5点)
2. Next.js の Image コンポーネントを使用して、画像 (`public/images/img.jpg`) をトップページに表示させてください。幅、高さはいずれも `144` を、代替文字列は「ロゴ」を指定してください。(5点)
3. Next.js の Link コンポーネントを使用して、トップページに「一覧画面」(`/users`) へのリンクを追加してください。リンクは、見出し 2「リンク」下の段落に挿入してください。(5点)

## 回答

以下は `pages/index.tsx` ファイルの内容です。

```typescript
import type { NextPage } from 'next';
import styles from '../styles/Home.module.css';
import Head from 'next/head';
import Image from 'next/image';
import Link from 'next/link';

const Home: NextPage = () => {
  return (
    <>
      <Head>
        <title>トップページ</title>
      </Head>
      <main className={styles.main}>
        <h1 className={styles.title}>トップページ</h1>

        <div>
          <Image
            src="/images/img.jpg"
            alt="ロゴ"
            width={144}
            height={144}
          />
        </div>

        <div className={styles.grid}>
          <a className={styles.card}>
            <h2>リンク &rarr;</h2>
            <p>
              <Link href="/users">一覧画面</Link>
            </p>
          </a>
        </div>
      </main>
    </>
  );
};

export default Home;
```

## 解説

### 1. タイトルをブラウザのタブに表示

目的:
- Webページのタイトルは、SEO（検索エンジン最適化）とユーザーエクスペリエンスに重要な役割を果たします。ページを開いた際に、ブラウザのタブにそのページの内容が一目で分かるようにするためにタイトルを設定します。

実装方法: 
- Next.jsでは、<Head>コンポーネントを使用してページの<head>セクションにメタ情報を追加します。以下のコードを使って、ページタイトルを「トップページ」に設定します。

```typescript
import Head from 'next/head';

<Head>
  <title>トップページ</title>
</Head>
```

`<title>` タグを使用して、ブラウザのタブに表示されるタイトルを「トップページ」に設定します。これにより、ユーザーがタブを切り替える際にページの内容がわかりやすくなります。

### 2. 画像を表示

Next.js の `Image` コンポーネントを使用して、最適化された画像を表示します。これにより、画像のパフォーマンスとロード時間が向上します。

```typescript
import Image from 'next/image';

<Image
  src="/images/img.jpg"
  alt="ロゴ"
  width={144}
  height={144}
/>
```

`Image` コンポーネントには、`src`、`alt`、`width`、`height` を指定します。`src` は画像のパス、`alt` は代替テキスト、`width` と `height` は画像の表示サイズです。このように指定することで、画像が正確なサイズで表示され、視覚的な効果が向上します。

- src: 画像ファイルのパス。
- alt: 代替テキスト。画像が表示されない場合やスクリーンリーダーでの読み上げ時に使用されます。
- widthとheight: 画像の表示サイズを指定します。

これらの指定により、画像は指定されたサイズで表示され、alt属性を使ってアクセシビリティも考慮されています。

### 3. リンクを追加

目的: 
- ページ内にリンクを設けることで、ユーザーが他のページに簡単に移動できるようにします。Next.jsのLinkコンポーネントを使うことで、クライアントサイドの高速なナビゲーションが可能となります。

実装方法: 
- 以下のようにLinkコンポーネントを使用します。

```typescript
import Link from 'next/link';

<p>
  <Link href="/users">一覧画面</Link>
</p>
```

- href: リンク先のパスを指定します。
- リンクテキスト: ユーザーに表示されるテキストを設定します。
これにより、ユーザーが「一覧画面」リンクをクリックすると、/usersページに移動します。このリンクはクライアントサイドで処理されるため、通常のHTMLリンクよりも素早くページ遷移が可能です。

## まとめ

この問題では、Next.js の基本的な機能である `Head` コンポーネント、`Image` コンポーネント、`Link` コンポーネントの使用方法について学びました。各コンポーネントは、SEO の改善、画像の最適化、クライアントサイドナビゲーションの実現に役立ちます。

問題を解く際には、各処理の役割と効果を理解しながら進めることが重要です。これにより、効率的で可読性の高いコードを作成できるようになります。
