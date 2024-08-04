# Next.js 問題・回答・解説

## 問題 4

SSR でユーザ詳細画面を実装します。以下の内容に沿ってファイル（`pages/users/ssr.tsx`）を編集してください。

- 上述のとおり、Server Side Rendering で実装する
- ページコンポーネントは実装されているものをそのまま使用する
- インポートされた非同期関数 `getUser` でユーザ情報を取得する

## 回答

以下は `pages/users/ssr.tsx` ファイルの内容です。

```typescript
import { GetServerSideProps, NextPage } from 'next';
import { getUser } from '../path/to/your/api';
import { User } from '../path/to/your/types';

type Props = {
  user: User;
};

const SSRUserPage: NextPage<Props> = ({ user }) => {
  return (
    <div>
      <h1>ユーザ詳細</h1>
      <p>名前: {user.name}</p>
      <p>メール: {user.email}</p>
    </div>
  );
};

export const getServerSideProps: GetServerSideProps = async ({ query }) => {
  // [ここから] 型ガードは採点対象外
  if (typeof query.id !== "string") {
    throw new TypeError("type of id is not string");
  }
  // [ここまで]

  const user = await getUser(parseInt(query.id));
  return {
    props: {
      user,
    },
  };
};

export default SSRUserPage;
```

## 解説

### 1. Server Side Rendering (SSR) の設定

Next.js では、`getServerSideProps` 関数を使用して、サーバーサイドでデータをフェッチし、そのデータをページコンポーネントに渡すことができます。以下のコードで `getServerSideProps` を設定し、ユーザ情報を取得します。

```typescript
export const getServerSideProps: GetServerSideProps = async ({ query }) => {
  if (typeof query.id !== "string") {
    throw new TypeError("type of id is not string");
  }

  const user = await getUser(parseInt(query.id));
  return {
    props: {
      user,
    },
  };
};
```

このコードは、クエリパラメータからユーザIDを取得し、`getUser` 関数を使用してユーザ情報をフェッチします。取得したデータは `props` としてページコンポーネントに渡されます。

### 2. ユーザ詳細の表示

フェッチしたデータを使用して、ユーザの詳細情報を表示するページコンポーネントを実装します。

```typescript
type Props = {
  user: User;
};

const SSRUserPage: NextPage<Props> = ({ user }) => {
  return (
    <div>
      <h1>ユーザ詳細</h1>
      <p>名前: {user.name}</p>
      <p>メール: {user.email}</p>
    </div>
  );
};
```

このコンポーネントは、`props` として受け取ったユーザデータを表示します。

## まとめ

この問題では、Next.js での Server Side Rendering を使用したデータフェッチ方法と、フェッチしたデータを使用したページコンポーネントの実装について学びました。SSR を使用することで、初回ページロード時に必要なデータをサーバーサイドで準備し、SEO やパフォーマンスの向上を図ることができます。
