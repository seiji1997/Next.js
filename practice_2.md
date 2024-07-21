
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

Next.js では、`Head` コンポーネントを使用して、ページの `<head>` セクションにメタ情報を追加できます。これにより、ページのタイトルを設定できます。

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

### 3. リンクを追加

Next.js の `Link` コンポーネントを使用して、クライアントサイドナビゲーションを実装します。これにより、ページ遷移が高速になります。

```typescript
import Link from 'next/link';

<p>
  <Link href="/users">一覧画面</Link>
</p>
```

`<Link>` コンポーネントの `href` 属性にリンク先のパスを指定し、リンクテキストを「一覧画面」に設定します。このリンクをクリックすることで、ユーザーは `/users` ページに移動できます。

## まとめ

この問題では、Next.js の基本的な機能である `Head` コンポーネント、`Image` コンポーネント、`Link` コンポーネントの使用方法について学びました。各コンポーネントは、SEO の改善、画像の最適化、クライアントサイドナビゲーションの実現に役立ちます。

問題を解く際には、各処理の役割と効果を理解しながら進めることが重要です。これにより、効率的で可読性の高いコードを作成できるようになります。

# Next.js 問題・回答・解説 (追加)

## 問題 2

新規登録画面 (`pages/users/new.tsx`) を編集してください。なお、新規登録画面はデータを必要としない静的なページです。

1. タイトル「登録画面」をブラウザのタブに表示させてください。(5点)
2. ターミナルから `yarn dev` を実行し、ブラウザで http://localhost:3000/users/new を開いてください。エラーが発生するので、エラーが解消するようにコードを修正してください。(10点)

## 回答

以下は `pages/users/new.tsx` ファイルの内容です。

```typescript
import { NextPage } from 'next';
import Head from 'next/head';
import { useRouter } from 'next/router';
import { SyntheticEvent, useState } from 'react';

const NewUserPage: NextPage = () => {
  const [name, setName] = useState('');
  const router = useRouter();

  const submitHandler = (ev: SyntheticEvent<HTMLFormElement>) => {
    ev.preventDefault();

    fetch(ev.currentTarget.action, {
      method: ev.currentTarget.method,
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ name }),
    })
      .then((res) => res.json())
      .then((data) => {
        router.push('/users');
      });
  };

  return (
    <>
      <Head>
        <title>登録画面</title>
      </Head>
      <form action="/api/users" method="post" onSubmit={submitHandler}>
        <fieldset>
          <legend>新規登録</legend>
          <p>
            名前:{' '}
            <input
              type="text"
              name="name"
              id="name"
              value={name}
              onChange={(e) => setName(e.target.value)}
            />
          </p>
          <button type="submit">登録</button>
        </fieldset>
      </form>
    </>
  );
};

export default NewUserPage;
```

## 解説

### 1. タイトルをブラウザのタブに表示

Next.js では、`Head` コンポーネントを使用して、ページの `<head>` セクションにメタ情報を追加できます。これにより、ページのタイトルを設定できます。

```typescript
import Head from 'next/head';

<Head>
  <title>登録画面</title>
</Head>
```

`<title>` タグを使用して、ブラウザのタブに表示されるタイトルを「登録画面」に設定します。これにより、ユーザーがタブを切り替える際にページの内容がわかりやすくなります。

### 2. フォームの実装とエラーハンドリング

Next.js のページにフォームを実装し、フォーム送信時にエラーハンドリングを行います。

```typescript
import { SyntheticEvent, useState } from 'react';
import { useRouter } from 'next/router';

const NewUserPage: NextPage = () => {
  const [name, setName] = useState('');
  const router = useRouter();

  const submitHandler = (ev: SyntheticEvent<HTMLFormElement>) => {
    ev.preventDefault();

    fetch(ev.currentTarget.action, {
      method: ev.currentTarget.method,
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({ name }),
    })
      .then((res) => res.json())
      .then((data) => {
        router.push('/users');
      });
  };
```

このコードでは、フォームの送信イベントを防ぎ、fetch API を使用してデータを送信します。送信後、ユーザーを `/users` ページにリダイレクトします。

### まとめ

この問題では、Next.js でのページタイトルの設定方法と、フォームの実装およびエラーハンドリングについて学びました。各処理の役割と効果を理解することで、効率的で可読性の高いコードを作成できるようになります。
