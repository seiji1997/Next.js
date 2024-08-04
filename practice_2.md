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

目的:
- ページタイトルは、SEO対策やユーザビリティ向上に重要です。適切なタイトルを設定することで、ブラウザのタブでページ内容が簡単に認識できます。

実装方法:
- Next.jsでは、<Head>コンポーネントを利用して、ページの<head>セクションにメタ情報を追加します。次のコードを使って、ページタイトルを「登録画面」に設定します。

```typescript
import Head from 'next/head';

<Head>
  <title>登録画面</title>
</Head>
```

`<title>` タグを使用して、ブラウザのタブに表示されるタイトルを「登録画面」に設定します。<br>
これにより、ユーザーがタブを切り替える際にページの内容がわかりやすくなります。<br>

2. フォームの実装とエラーハンドリング
目的:
- フォームを実装し、ユーザー入力を適切に処理します。フォーム送信時にエラーが発生しないようにし、ユーザーを正しいページにリダイレクトする機能を追加します。

実装方法:
- 以下の手順でフォームを実装します。

1, ステートの設定:
- フォームの入力値を管理するために、useStateフックを使用してステートを定義します。

```typescript
import { useState } from 'react';

const [name, setName] = useState('');
```

nameというステートを定義し、フォーム入力の値を管理します。

2, ルーターの設定:
- Next.jsのuseRouterフックを使用して、ページ遷移を行います。

```typescript
import { useRouter } from 'next/router';
const router = useRouter();
```

3, フォームの送信ハンドラーの設定:
- フォームの送信を処理するために、submitHandlerを定義します。この関数は、フォームのデフォルトの送信動作を防ぎ、fetch APIを使ってデータを送信します。

```typescript
import { SyntheticEvent } from 'react';

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
    .then(() => {
      router.push('/users');
    });
};
```

- ev.preventDefault(): フォームのデフォルトの送信動作を防ぎます。
- fetch: フォームの送信先URL（action）とHTTPメソッド（method）を使用して、サーバーにデータを送信します。
- JSON.stringify({ name }): フォームデータをJSON形式で送信します。
- router.push('/users'): データ送信後、/usersページにリダイレクトします。

フォームの実装:
- フォームコンポーネントを作成し、submitHandlerをonSubmitイベントに設定します。

```typescript
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
```

- \<input>: 入力フィールドを定義し、value属性をnameステートにバインドします。onChangeイベントで入力値を更新します。
- \<button>: フォームの送信ボタンです。

#### まとめ
この問題では、Next.jsのHeadコンポーネントを使ったタイトル設定、フォームの実装、エラーハンドリングについて学びました。各要素を適切に組み合わせることで、ユーザーフレンドリーで効率的なWebページを構築できます。<br>

フォーム実装の際には、各ステートの管理、イベントハンドリング、ページ遷移の処理に注意を払い、コードの可読性とメンテナンス性を高めることが重要です。これにより、開発効率が向上し、より良いユーザーエクスペリエンスを提供できるようになります。
