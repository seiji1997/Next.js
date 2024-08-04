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
