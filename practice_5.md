# Next.js 問題・回答・解説

## 問題 5

SSG と動的ルーティングでユーザ詳細画面を実装します。以下の内容に沿ってファイル（`pages/users/ssg/[id].tsx`）を編集してください。

- インポートされた同期関数 `getActiveUsers` で全ユーザ情報を取得する
- インポートされた非同期関数 `getUser` で個別のユーザ情報を取得する
- ページコンポーネントは実装されているものをそのまま使用する

## 回答

以下は `pages/users/ssg/[id].tsx` ファイルの内容です。

```typescript
import { GetStaticPaths, GetStaticProps, NextPage } from 'next';
import { getActiveUsers, getUser } from '../path/to/your/api';
import { User } from '../path/to/your/types';

type Props = {
  user: User;
};

const SSGUserPage: NextPage<Props> = ({ user }) => {
  return (
    <div>
      <h1>ユーザ詳細</h1>
      <p>名前: {user.name}</p>
      <p>メール: {user.email}</p>
    </div>
  );
};

export const getStaticPaths: GetStaticPaths = async () => {
  const users = await getActiveUsers();
  return {
    paths: users.map((user) => ({
      params: {
        id: user.id.toString(),
      },
    })),
    fallback: false,
  };
};

export const getStaticProps: GetStaticProps = async ({ params }) => {
  // [ここから] 型ガードは採点対象外
  if (!params) {
    throw new TypeError("params is undefined");
  }

  if (typeof params.id !== "string") {
    throw new TypeError("type of id is not string");
  }
  // [ここまで]

  const user = await getUser(parseInt(params.id));
  return {
    props: {
      user,
    },
  };
};

export default SSGUserPage;
```

## 解説

### 1. 動的ルーティングと静的生成 (SSG) の設定

目的:
- Next.jsのSSG機能を活用して、動的なユーザ詳細ページを静的に生成します。動的ルーティングにより、ユーザIDに基づいて個別のページを生成します。
- Next.js では、`getStaticPaths` 関数を使用して、事前に生成するパスを定義します。以下のコードで `getStaticPaths` を設定し、全ユーザ情報を取得して動的パスを生成します。

```typescript
export const getStaticPaths: GetStaticPaths = async () => {
  const users = await getActiveUsers();
  return {
    paths: users.map((user) => ({
      params: {
        id: user.id.toString(),
      },
    })),
    fallback: false,
  };
};
```

このコードは、`getActiveUsers` 関数を使用して全ユーザ情報を取得し、その情報から動的パスを生成します。`fallback` を `false` に設定することで、定義されたパス以外のリクエストは 404 ページが返されます。

- getStaticPaths: この関数は、Next.jsに生成する必要があるすべてのパスを指定します。
  - getActiveUsers: ユーザ情報を取得する同期関数を呼び出して、全てのユーザのIDをリストアップします。
  - paths: 生成するパスをユーザのIDに基づいて設定します。
  - fallback: false: 指定されたパス以外のページは404エラーを返します。


### 2. ユーザ情報の取得

目的:
- getStaticPropsを使用して、特定のユーザのデータをビルド時にフェッチし、ページに必要なデータを準備します。
- `getStaticProps` 関数を使用して個別のユーザ情報を取得します。

```typescript
export const getStaticProps: GetStaticProps = async ({ params }) => {
  if (!params) {
    throw new TypeError("params is undefined");
  }

  if (typeof params.id !== "string") {
    throw new TypeError("type of id is not string");
  }

  const user = await getUser(parseInt(params.id));
  return {
    props: {
      user,
    },
  };
};
```

このコードは、クエリパラメータからユーザIDを取得し、`getUser` 関数を使用してユーザ情報をフェッチします。取得したデータは `props` としてページコンポーネントに渡されます。

- getStaticProps: この関数は、静的生成時にデータを取得し、そのデータをコンポーネントにpropsとして渡します。
  - params: 動的ルーティングのパラメータから取得したユーザID。
  - getUser: 非同期関数として呼び出し、特定のユーザデータを取得します。
  - props: 取得したユーザデータをページコンポーネントに渡します。

### 3. ユーザ詳細の表示

目的:
- 取得したユーザ情報を使用して、ユーザの詳細を表示します。
- フェッチしたデータを使用して、ユーザの詳細情報を表示するページコンポーネントを実装します。

```typescript
type Props = {
  user: User;
};

const SSGUserPage: NextPage<Props> = ({ user }) => {
  return (
    <div>
      <h1>ユーザ詳細</h1>
      <p>名前: {user.name}</p>
      <p>メール: {user.email}</p>
    </div>
  );
};
```

コンポーネント: SSGUserPageは、propsとして渡されたユーザデータを使用してユーザの詳細を表示します。

## まとめ

この問題では、Next.jsでの静的生成(SSG)と動的ルーティングの実装について学びました。SSGを使用することで、以下の利点があります:

- パフォーマンス: ビルド時にページが生成されるため、リクエスト時には非常に高速にページを提供できます。
- SEO: サーバー側でレンダリングされたコンテンツが提供されるため、検索エンジンがコンテンツを正確にインデックスできます。
- スケーラビリティ: 動的ルーティングにより、無数のページを効率的に管理できます。

SSGと動的ルーティングを活用することで、ユーザーに対して迅速で信頼性の高いWebエクスペリエンスを提供でき、特に規模の大きいアプリケーションでの効率的なコンテンツ配信が可能になります。