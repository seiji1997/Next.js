# Next.js 問題・回答・解説

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
