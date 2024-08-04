# Next.js 問題・回答・解説

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

目的:
- SWRは、React Hooksを使用したデータフェッチングライブラリであり、キャッシュとリアルタイムなデータ更新を簡単に管理できます。この問題では、ユーザー情報をAPIから取得し、画面に表示します。

```typescript
import useSWR from 'swr';
import { useRouter } from 'next/router';

const fetcher = (url: string) => fetch(url).then((res) => res.json());
```

- useSWRをインポートし、データフェッチのためのfetcher関数を定義します。
- fetcher関数は指定されたURLからデータをフェッチし、JSONとして解析します。

### 2. データフェッチとエラーハンドリング

目的:
- ユーザーIDに基づいてデータを取得し、エラーまたはロード状態に応じた表示を行います。
- `useSWR` フックを使用してデータをフェッチします。

```typescript
const { data, error } = useSWR<User>(
  query.id ? `/api/users/${query.id}` : null,
  fetcher
);

if (error) return <p>エラー</p>;
if (!data) return <p>ロード中…</p>;
```

- useSWRフックを使用して、データ、エラー、およびロード状態を取得します。
- ユーザーIDがquery.idとして存在する場合のみ、データをフェッチします。存在しない場合、nullを返します。
- errorが存在する場合は「エラー」、データがまだフェッチされていない場合は「ロード中…」を表示します。

### 3. ユーザ詳細の表示

目的: 
- データが正常にフェッチされた場合、ユーザの詳細情報を表示します。

```typescript
return (
  <div>
    <h1>ユーザ詳細</h1>
    <p>名前: {data.name}</p>
    <p>メール: {data.email}</p>
  </div>
);
```

- データが正常にフェッチされた場合、dataオブジェクトからユーザーのnameとemailを取り出し、画面に表示します。

## まとめ

この問題では、Next.jsでのCSRによるデータフェッチ方法、エラーハンドリング、ロード状態の管理について学びました。SWRを活用することで、効率的かつ簡潔にクライアントサイドでのデータフェッチを実装できます。これにより、リアルタイムでのデータ更新やキャッシュの管理が容易になり、ユーザーエクスペリエンスが向上します。

ポイント:
- CSR: クライアントサイドでのレンダリングにより、初期ロードが軽量で素早く、ユーザーのインタラクションに対して迅速に応答します。
- SWR: キャッシュ機構とリアルタイムのデータ更新が簡単に実装でき、開発効率を高めるライブラリです。
- エラーハンドリング: フェッチ中の状態やエラーの状態を適切にユーザーに通知し、ユーザーエクスペリエンスを向上させます。

これらの知識を活用することで、Next.jsアプリケーションにおいて効率的なデータ管理とユーザビリティを実現できます。