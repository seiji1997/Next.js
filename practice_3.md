
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

# Next.js 問題・回答・解説 (追加)

## 問題 3

CSR でユーザ詳細画面を実装します。以下の内容に沿ってファイル（`pages/users/csr.tsx`）を編集してください。

- SWR フックを使う
- ユーザ ID が 1 のユーザ情報は、`/api/users/1` から取得できます
- ユーザ情報を取得する際にエラーになった場合は「エラー」を画面に表示
- ユーザ情報がまだ取得できていない場合は「ロード中…」を画面に表示

## 回答

以下は `pages/users/csr.tsx` ファイルの内容です。

```typescript
import { NextPage } from 'next';
import useSWR from 'swr';
import { useRouter } from 'next/router';

const fetcher = (url: string) => fetch(url).then((res) => res.json());

const CSRUserPage: NextPage = () => {
  const router = useRouter();
  const query = router.query;

  const { data, error } = useSWR<User>(
    query.id ? `/api/users/${query.id}` : null,
    fetcher
  );

  if (error) return <p>エラー</p>;
  if (!data) return <p>ロード中…</p>;

  return (
    <div>
      <h1>ユーザ詳細</h1>
      <p>名前: {data.name}</p>
      <p>メール: {data.email}</p>
    </div>
  );
};

export default CSRUserPage;
```

## 解説

### 1. SWR フックを使用してデータを取得

SWR は React Hooks を使用したデータフェッチングライブラリです。以下のコードで SWR をインポートし、データを取得します。

```typescript
import useSWR from 'swr';
import { useRouter } from 'next/router';

const fetcher = (url: string) => fetch(url).then((res) => res.json());
```

`fetcher` 関数は指定された URL からデータをフェッチし、JSON として解析します。

### 2. データフェッチとエラーハンドリング

次に、`useSWR` フックを使用してデータをフェッチします。

```typescript
const { data, error } = useSWR<User>(
  query.id ? `/api/users/${query.id}` : null,
  fetcher
);

if (error) return <p>エラー</p>;
if (!data) return <p>ロード中…</p>;
```

`useSWR` フックは、データ、エラー、およびロード状態を返します。`query.id` が存在しない場合、`null` を返すようにします。

### 3. ユーザ詳細の表示

データが正常にフェッチされた場合、ユーザの詳細情報を表示します。

```typescript
return (
  <div>
    <h1>ユーザ詳細</h1>
    <p>名前: {data.name}</p>
    <p>メール: {data.email}</p>
  </div>
);
```

このコードは、フェッチされたユーザの名前とメールアドレスを表示します。

## まとめ

この問題では、Next.js での CSR を使用したデータフェッチ方法、エラーハンドリング、およびロード状態の管理について学びました。SWR を使用することで、効率的で簡潔なデータフェッチロジックを実装できます。