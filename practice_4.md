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

目的:
- SSRを使用することで、ページの初期ロード時にサーバー側でデータを取得し、そのデータをページコンポーネントに渡すことができます。これにより、SEO対策やユーザーがページに訪れたときのデータ取得を最適化できます。
- Next.js では、`getServerSideProps` 関数を使用して、サーバーサイドでデータをフェッチし、そのデータをページコンポーネントに渡すことができます。以下のコードで `getServerSideProps` を設定し、ユーザ情報を取得します。

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
- getServerSideProps: これはNext.jsが提供する関数で、リクエストごとにサーバーサイドで実行され、データをフェッチします。
- 型ガード: query.idが文字列であることを確認しています。これにより、型安全性を高め、不正なデータ型によるエラーを防ぎます。
- getUser: 非同期関数としてインポートされ、ユーザIDを引数にユーザ情報を取得します。

### 2. ユーザ詳細の表示

目的:
- サーバーサイドで取得したユーザデータを使用して、ユーザ詳細情報を表示します。

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

- コンポーネント: SSRUserPageは、サーバーサイドで取得したユーザデータを受け取り、そのデータを表示します。
- props: コンポーネントに渡されるpropsには、ユーザのnameとemailが含まれます。

## まとめ

この問題では、Next.jsを使用したServer Side Rendering (SSR) の基本的な実装方法について学びました。SSRを使用することで、以下の利点があります:

- SEO向上: サーバーサイドでデータをレンダリングするため、クローラーがコンテンツを適切にインデックス化できます。
- パフォーマンス: 初期レンダリング時に必要なデータをすでに取得しているため、ユーザーは完全にレンダリングされたページを即座に見ることができます。
- セキュリティ: データをサーバーサイドで処理するため、クライアントに送信する前に適切なバリデーションやエラーハンドリングを行うことができます。

SSRの活用により、SEOやユーザビリティを考慮した効果的なWebアプリケーションを構築することが可能です。これにより、ユーザーにとってスムーズで迅速なアクセスを提供できるようになります。