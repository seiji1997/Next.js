# Next.js 問題・回答・解説

## 問題 6

ユーザー一覧画面を CSR で実装します。ファイル（`pages/users/index.tsx`）を以下の内容に沿って編集してください。

- SWR フックを使う
- ユーザー一覧は「/api/users」で取得できる
- ユーザー情報を取得する際にエラーになった場合は「エラー」を画面に表示
- ユーザー情報が取得できていない場合は「ロード中...」を画面に表示
- ユーザーごとに、編集ボタンの横に削除ボタンを追加
- ID が 1 のユーザーであれば、HTTP「DELETE」メソッドで「/api/users/1」にリクエストを送信すると削除されます
- ユーザーの削除が成功した際に、一覧表示を更新する

## 回答

以下は `pages/users/index.tsx` ファイルの内容です。

```typescript
import { NextPage } from "next";
import Head from "next/head";
import Link from "next/link";
import { useRouter } from "next/router";
import useSWR, { Fetcher, useSWRConfig } from "swr";

type User = {
  id: number;
  name: string;
};

const fetcher: Fetcher<User[]> = (resource: string) =>
  fetch(resource).then((res) => res.json());

const UsersPage: NextPage = () => {
  const router = useRouter();
  const { mutate } = useSWRConfig();
  const { data, error } = useSWR("/api/users", fetcher);

  if (error) return <p>エラー</p>;
  if (!data) return <p>ロード中...</p>;

  return (
    <>
      <Head>
        <title>一覧画面</title>
      </Head>
      <div>
        <Link href={"/users/new"} legacyBehavior>
          <a>新規登録</a>
        </Link>
      </div>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>名前</th>
            <th>リンク</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          {data.map((user) => {
            return (
              <tr key={user.id}>
                <td>{user.id}</td>
                <td>{user.name}</td>
                <td>
                  <Link href={`/users/ssg/${user.id}`} legacyBehavior>
                    <a>SSGで表示</a>
                  </Link>
                  {' | '}
                  <Link href={`/users/ssr?id=${user.id}`} legacyBehavior>
                    <a>SSRで表示</a>
                  </Link>
                  {' | '}
                  <Link href={`/users/csr?id=${user.id}`} legacyBehavior>
                    <a>CSRで表示</a>
                  </Link>
                </td>
                <td>
                  <button
                    type="button"
                    onClick={() => {
                      router.push(`/users/edit?id=${user.id}`);
                    }}
                  >
                    編集
                  </button>
                  {' | '}
                  <button
                    type="button"
                    onClick={() => {
                      fetch(`/api/users/${user.id}`, {
                        method: "DELETE",
                      }).then((res) => {
                        if (res.ok) {
                          mutate("/api/users");
                        }
                      });
                    }}
                  >
                    削除
                  </button>
                </td>
              </tr>
            );
          })}
        </tbody>
      </table>
    </>
  );
};

export default UsersPage;
```

## 解説

### 1. SWR フックを使用してデータを取得

目的:
- SWRは、データのフェッチとキャッシュ管理を簡素化するためのReact Hooksを使用したライブラリです。この問題では、ユーザーの一覧を取得し、エラーハンドリングとロード状態を管理します。
- SWR は React Hooks を使用したデータフェッチングライブラリです。以下のコードで SWR をインポートし、データを取得します。

```typescript
import useSWR, { Fetcher, useSWRConfig } from "swr";

type User = {
  id: number;
  name: string;
};

const fetcher: Fetcher<User[]> = (resource: string) =>
  fetch(resource).then((res) => res.json());
```

`fetcher` 関数は指定された URL からデータをフェッチし、JSON として解析します。
- fetcher: 指定されたURLからデータをフェッチし、JSON形式に解析します。
- useSWR: データの取得、エラー処理、ロード状態を管理します。

### 2. データフェッチとエラーハンドリング

目的:
- ユーザー一覧をAPIから取得し、エラーやロード状態に応じた表示を行います。
- 次に、`useSWR` フックを使用してデータをフェッチします。

```typescript
const { data, error } = useSWR("/api/users", fetcher);

if (error) return <p>エラー</p>;
if (!data) return <p>ロード中...</p>;
```

`useSWR` フックは、データ、エラー、およびロード状態を返します。`/api/users` エンドポイントからユーザ情報を取得します。
- エラーハンドリング: errorが存在する場合には「エラー」を表示。
- ロード状態: データがまだ取得されていない場合には「ロード中...」を表示。


### 3. ユーザ情報の表示と削除機能の実装

目的:
- 取得したデータを使用してユーザーの一覧を表示し、各ユーザーに編集および削除機能を追加します。
- フェッチしたデータを使用して、ユーザの一覧情報を表示し、編集および削除機能を実装します。

```typescript
return (
  <>
    <Head>
      <title>一覧画面</title>
    </Head>
    <div>
      <Link href={"/users/new"} legacyBehavior>
        <a>新規登録</a>
      </Link>
    </div>
    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>名前</th>
          <th>リンク</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        {data.map((user) => {
          return (
            <tr key={user.id}>
              <td>{user.id}</td>
              <td>{user.name}</td>
              <td>
                <Link href={`/users/ssg/${user.id}`} legacyBehavior>
                  <a>SSGで表示</a>
                </Link>
                {' | '}
                <Link href={`/users/ssr?id=${user.id}`} legacyBehavior>
                  <a>SSRで表示</a>
                </Link>
                {' | '}
                <Link href={`/users/csr?id=${user.id}`} legacyBehavior>
                  <a>CSRで表示</a>
                </Link>
              </td>
              <td>
                <button
                  type="button"
                  onClick={() => {
                    router.push(`/users/edit?id=${user.id}`);
                  }}
                >
                  編集
                </button>
                {' | '}
                <button
                  type="button"
                  onClick={() => {
                    fetch(`/api/users/${user.id}`, {
                      method: "DELETE",
                    }).then((res) => {
                      if (res.ok) {
                        mutate("/api/users");
                      }
                    });
                  }}
                >
                  削除
                </button>
              </td>
            </tr>
          );
        })}
      </tbody>
    </table>
  </>
);
```

このコードは、ユーザの一覧情報を表示し、各ユーザごとに編集および削除ボタンを追加します。削除ボタンをクリックすると、`/api/users/{id}` エンドポイントに DELETE リクエストが送信され、成功した場合はユーザ一覧が更新されます。
- 編集ボタン: router.pushを使用して、編集ページに遷移します。
- 削除ボタン: fetchを使用してHTTP DELETEメソッドでリクエストを送信し、削除が成功した場合はmutateを呼び出して一覧を更新します。

## まとめ

この問題では、Next.jsでのCSRを使用したデータフェッチ、エラーハンドリング、削除機能の実装について学びました。SWRを使用することで、以下の利点があります:

- 効率的なデータ管理: クライアントサイドでリアルタイムにデータを取得し、更新できます。
- キャッシュ機構: データのフェッチが効率化され、パフォーマンスが向上します。
- 簡単な実装: 少ないコードで効果的なデータ管理とUI更新を実現できます。

CSRを活用することで、ユーザーに対して迅速でインタラクティブなエクスペリエンスを提供し、リアルタイムでのデータ更新を実現できます。これにより、ユーザー管理アプリケーションが効率的に機能し、スケーラブルなソリューションを提供できます。